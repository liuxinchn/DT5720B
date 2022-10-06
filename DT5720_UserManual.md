# DT5720_UserManual

>- User Manual UM3244
>- rev10
>- 2017.05.26 
>- 2/4 Channel 12bit 250MS/s Digitizer



## 目的

1. 完整硬件描述
2. 作为波形记录数字化仪Waveform Recording Digitizer（后称波形记录固件Waveform recording firmware）的工作原理

firmware version: 4.14_0.14

- 寄存器的去看 UM5961 – 720 Registers Description.
- DPP(Digital Pulse Processing) firmware，去看  UM2088 – DPP-PSD User Manual.



## 符号

ADC 	   Analog-to-Digital Converter
AMC 	  ADC & Memory Controller
DAQ 	  Data Acquisition
DAC 	   Digital-to-Analog Converter
DC 		 Direct Current
LVDS 	 Low-Voltage Differential Signal
PLL 		Phase-Locked Loop
ROC 	   ReadOut Controller
TTT 		Trigger Time Tag
USB 	   Universal Serial Bus



## Introduction

DT5720: 2/4 Channel 12 bit 250 MS/s Desktop Waveform Digitizer with 2 Vpp input dynamic range

(Vpp： 峰峰值，最大正弦值 - 最小正弦值)

（MS/s: S=sample s=second M=10^6 表示每秒钟的采样数）

**Considering the sampling frequency and bit number, these digitizers are well suited for mid-fast signals**  as the ones coming from liquid or inorganic scintillators coupled with PMTs or Silicon Photomultiplie

each channel is able to generate a self-trigger when the input signal goes under/over a programmable threshold.

During the acquisition, data stream is continuously written in a circular memory buffer.

Two modes are supported for the event storage in the board memories: Standard mode and Pack2.5 mode

The board houses USB 2.0 and optical link interfaces. USB 2.0 allows data transfers up to 30 MB/s.  The
Optical Link interface is capable of transfer rate up to 80 MB/s and offers daisy chain capability.

In addition to the waveform recording firmware, CAEN provides for this digitizer the Digital Pulse Processing firmware (DPP) for the Pulse Shape Discrimination (DPP-PSD) , which combines the functionalities of a digital QDC (charge integration) and discriminator of different shapes for particle identification.



## Block Diagram

![image-20220928104445956](./fig/blockdiagram.png)



## 技术规格





## Board Description

### Front Panel

![Font Panel View](./fig/frontPanel.jpg)

| Connector         | Function                                                     |
| ----------------- | ------------------------------------------------------------ |
| CH0~CH3           | receive the input analog signals.                            |
| CLK IN            | Input and output connectors for the external clock.          |
| GPO               | General purpose programmable digital output connector to propagate:<br/>• the internal trigger sources; |
| TRG-IN            | Digital input connector for the external trigger.            |
| GPI               | General purpose programmable input connector.                |
| OPTICAL LINK PORT | Optical LINK connector for data readout and flow control. Daisy chainable. |
| USB PORT          | USB connector for data readout and flow control.             |
| DIAGNOSTICS LEDs  |                                                              |

### Rear Panel

![Rear Panel View](./fig/rearPanel.jpg)

| Connector         | Function                                                     |
| ----------------- | ------------------------------------------------------------ |
| SPARE LINK        | Auxiliary connector reserved for CAEN usage.                 |
| DC INPUT          | Input connector for the desktop Digitizer<br/>main power supply from the external AC/DC adapter. |
| IDENTIFYING LABEL | A blue label on the Desktop rear panel indicates             |



## Functional Description

### Analog Input Stage

Input dynamic is 2 Vpp. In order to preserve the full dynamic range with unipolar input signal, positive
or negative, it is possible to add a DC offset by means of a 16 bit DAC, which is up to +- 1 V.

![Analog input diagram](C:\Users\49411\AppData\Roaming\Typora\typora-user-images\image-20220928161857634.png)

### Clock Distribution

The clock distribution of the module takes place on two domains: OSC-CLK and REF-CLK.

OSC-CLK is a fixed 50-MHz clock coming from a local oscillator which handles USB, Optical Link and Local
Bus



### Trigger Clock

The TRG-CLK logic works at 125 MHz, equal to the sampling frequency: TRG-CLK = SAMPL-CLK.



### Acquisition Modes



#### Acquisition Run/Stop

有三种方式来Run/Stop

- 软件指定
- GPI signal
- TRG-IN，the first trigger pulse



#### Acquisition Triggering: Samples and Events

一个Event由 触发时间标识、触发前采样、触发后采样 和 事件计数组成。

”采集窗口“会发生重叠，这种重叠可以被拒绝也可以被接受，可以通过软件编程实现。

![image-20220928205118909](C:\Users\49411\AppData\Roaming\Typora\typora-user-images\image-20220928205118909.png)

#### Multi-Event Memory Organization

每个通道都有单独的SRAM内存，内存可以再划分成不同数量的缓冲区。

自定义缓冲区大小直接影响采集窗口的宽度，CAEN中记录长度参数和CAENDigitizer库中的Set/GetRecordlength()函数都依赖于这些概念。



### Event structure

事件由Header和Data组成。数据格式32位字长

![image-20220929103158565](C:\Users\49411\AppData\Roaming\Typora\typora-user-images\image-20220929103158565.png)



### Acquisition Synchronization

每个通道都有SRAM内存，内存分成多个缓冲区，当trigger发生时，FPGA会进一步为post-trigger写入可编程数量的样本并冻结缓冲区，以便存储的数据可以通过USB或Optical Link读取。Acquisiton可以在新的缓冲区中继续进行。

所有buffer都写满之后，board被认为时Full，这时候不能再被写入，并停止采集，直到有一个buffer被读出，此时board推出full。



### Zero Suppression

x720可以根据 Zero Suppression去选择事件，Zero Suppression允许用户以只传输有用数据的方式来减少数据传输的数据量。但是Zero Suppression在读出数据时起作用，所有存在延迟，并且所有的事件必须使用相同的trigger，由FPGA分析满足条件的事件并传输它。



#### Full Suppression based on the Amplitude of the Signal

两种方法丢弃数据

1. 信号没有超过了阈值至少N秒
2. 信号没有超过阈值



阈值Tamp和Ns是个人设置的，Novt是实际超过阈值的时间，Novt > Ns，事件被获取。

![image-20220929163121761](C:\Users\49411\AppData\Roaming\Typora\typora-user-images\image-20220929163121761.png)

#### Zero Length Encoding ZLE

会把样本标记为“good”和“skipped”，交错记录，所有的数据都会被记录下来（包括skipped，但skipped只记录个数，不会详细记录波形信息）。

![image-20221004093909587](C:\Users\49411\AppData\Roaming\Typora\typora-user-images\image-20221004093909587.png)



### Trigger Management

**Software Trigger:** 通过软件命令（途经USB或Optical Link）

**External Trigger：**the front panel TRG-IN connector

**Self-Trigger:**设置阈值与N，当连续N个点超过阈值，开始采集。[self-Trigger具有延迟性，因为要连续采N个点]

**Coincidences:**对于所有通道来说，设置一个Majority Level，当 `Number of enabled channels > Majority level `，可以采集。

**TRG-IN as Gate：**the AND between the external signal on TRG-IN and the other trigger sources



### Data Transfer Capabilities and Events Readout

Event size是可以配置的。

**Block Transfer:**

1. 使用中断
2. 使用轮询，对特定寄存器进行周期性访问
3. 使用连续读取

### Optical Link and USB Access

usb2.0的传输率是30MB/s，daisy chainable Optical Link的传输率为80MB/s。



## Drivers & Libraries

CAEN库是CAEN软件工具正确运行所需的一组中间件软件。

这些库为那些希望为数字化器控件开发定制应用程序(通信、配置、读取等)的用户提供了强大的基础。

- CAENDigitizer：是专门为数字化仪设计的函数库，支持波形记录固件和DPP固件。CAENDigitizer库是基于CAENComm库的。因此，在安装CAENDigitizer之前，必须在主机PC上已经安装了CAENComm库。
- CAENComm：管理低层通信(读和写访问)。CAENComm的目的是实现到较高级软件层的公共接口，屏蔽物理通道及其协议的细节，从而使依赖于CAENComm的库和应用程序独立于物理层。此外，即使在不使用VME的情况下，CAENComm也需要CAENVMELib库(访问VME总线)。这就是为什么CAENVMELib必须在安装CAENComm之前已经安装在您的PC上的原因。

![驱动和软件层](C:\Users\49411\AppData\Roaming\Typora\typora-user-images\image-20221005094859152.png)



## Software Tools



### CAENUpgrader

由命令行和Java图形用户界面组成，主要是管理固件的，它可以：

- 在数字化仪上上传固件
- 读取固件版本
- 管理固件许可证
- 升级内部的PLL
- 获取Board Info文件（支持的情况下）

前提是安装CAENVMELib和CAENComm，并且Java SE 8 update 40以上。



### CAENComm Demo

CAENComm Demo是用C/ c++源代码开发的简单软件，提供Java™和LabVIEW™GUI界面。该演示主要通过对寄存器的直接读/写访问在低级别上允许全板配置，并可以用作调试仪器。



### CAEN WareDump

WaveDump是一个基本的控制台应用程序，没有图形，只支持运行波形记录固件的CAEN数字化程序。它允许用户对单个板进行编程(根据包含参数和指令列表的文本配置文件)，启动/停止采集，读取数据，显示读出和触发率，应用一些后处理(例如FFT和振幅直方图)，将数据保存到文件，并使用Gnu plot(第三方绘图实用程序:www.gnuplot.info)绘制波形。

WaveDump是一个非常有用的C代码示例，演示了如何使用库和方法进行高效的读取和数据分析。感谢所包含的源文件和VS项目，强烈建议所有愿意自己编写软件的用户从这个演示开始。

注意：CAEN WaveDump可操作Windows®和Linux®平台(32位和64位);该软件依赖于CAENDigitizer, CAENComm和CAENVMELib库。Windows®版本的WaveDump是独立的(所有所需的库都包含在软件包中)，而Linux®版本需要用户预先安装所需的库。此外，Linux®用户还需要安装第三方Gnu plot。



### CAEN Scope

在一个全新的框架中，CAENScope软件允许管理运行波形记录固件的CAEN数字化器。

CAENScope用户友好的界面提供了不同的部分，方便地管理数字化配置和绘制波形。一旦连接，程序检索数字化器信息。可以为通道设置不同的参数，触发器和轨迹可视化(多达12条轨迹)可以同时绘制。信号可以以两种不同的格式记录到文件中:二进制(SQLite db)和文本(XML)。也可以保存和恢复程序设置。

注意:Windows®和Linux®版本是独立的。该软件下载所需的CAENDigitizer, CAENComm和CAENVMELib库。

Linux用户需要安装以下软件包:

- sharutils;
- libXft;
- libXss(专门用于Debian派生的发行版，例如Debian, Ubuntu等);
- libXScrnSaver(专门用于Red Hat派生的发行版，例如RHEL、Fedora、Centos等)。



### DPP-PSD Control Software 

DPP-PSD控制软件是一个演示应用程序，介绍用户了解脉冲形状识别的数字脉冲处理(DPP-PSD)的操作原理。它可以管理运行DPP-PSD固件和DT5790数字脉冲分析仪的CAEN 720, 725, 730和751数字仪系列的单板通信和采集。

DPP- psd控制软件基于用于参数设置(连接、DPP算法、采集等)的Java图形用户界面，一个C控制台应用程序作为采集引擎(DPPRunner)和一个第三方图形实用程序(Gnuplot: www.gnuplot.info)。GUI通过运行时命令直接处理采集引擎，并生成包含所有所选参数值的文本配置文件。这个文件由DPPRunner读取，DPPRunner根据参数对Digitizer进行编程，启动采集并管理数据读出。

该软件可以在示波器模式下运行，其中来自内部滤波器的数字化输入波形和数字信号被监控，以便更好地调优DPP参数，在直方图模式下，其中能量(即电荷)和时间直方图(由软件构建)可以被监控。

根据工作模式，波形或电荷、PSD和时间戳列表以及能量或时间直方图等原始数据可以保存到输出文件中，用于离线分析。



### CoMPASS

CoMPASS (CAEN多参数光谱软件)是来自CAEN的新软件，能够实现物理应用的多参数DAQ，其中探测器可以直接连接到数字化输入，软件获得能量，定时和PSD光谱。

CoMPASS软件被设计成一个用户友好的界面，以管理所有CAEN DPP算法的采集。CoMPASS可以管理多个板，甚至在同步模式下，和不同通道(硬件和/或软件)之间的事件相关性，应用能量和PSD切割，计算和显示统计数据(触发率，数据吞吐量等…)，保存输出数据文件(原始数据，列表，波形，频谱)，并使用保存的文件与不同的处理参数脱机运行。

CoMPASS软件支持运行DPPPSD、DPP-PHA和DPP-QDC固件的CAEN x720、x724、x725、x730、x740D、x751数字化器家族和x781 MCA家族
