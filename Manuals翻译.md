The Mod. DT5720 is 2/4 Channel 12 bit 250 MS/s Desktop Waveform Digitizer with 2 Vpp input dynamic range on single ended MCX coaxial connectors (see Tab. 1.1). The DC offset is adjustable via a 16-bit DAC on each channel in the 1 V range. 
Considering the sampling frequency and bit number, these digitizers are well suited for mid-fast signals as the ones coming from liquid or inorganic scintillators coupled with PMTs or Silicon Photomultiplier. 
A common acquisition trigger signal (common to all the channels) can be fed externally via the front panel TRG-IN input connector or via software. Alternatively, each channel is able to generate a self-trigger when the input signal goes under/over a programmable threshold. The trigger from one board can be propagated out of the board through the front panel GPO . 
During the acquisition, data stream is continuously written in a circular memory buffer. When the trigger occurs, the digitizer writes further samples for the post trigger and freezes the buffer that can be read by one of the provided readout links. 
Each channel has a SRAM digital memory (see Tab. 1.1 for the available memory size options) divided into buffers of programmable size (1/1024). The readout (from USB or Optical link) of a frozen buffer is independent from the write operations in the active circular buffer (ADC data storage). 
Two modes are supported for the event storage in the board memories: Standard mode and Pack2.5 mode (see Sec. Event structure). 
DT5720 features front panel CLK-IN connector as well as an internal PLL for clock synthesis (50 MHz oscillator) from internal/external references. 
The board houses USB 2.0 and optical link interfaces. USB 2.0 allows data transfers up to 30 MB/s.The Optical Link interface (CAEN proprietary CONET protocol) is capable of transfer rate up to 80 MB/s and offers daisy chain capability. Therefore, it is possible to connect up to 8 ADC modules to a single A2818 Optical Link Controller, or up to 32 using a 4-link A3818 version (Mod. A2818/A3818, see Tab. 1.1). 
In addition to the waveform recording firmware, CAEN provides for this digitizer the Digital Pulse Processing firmware (DPP) for the Pulse Shape Discrimination (DPP-PSD) [RD2], which combines the functionalities of a digital QDC (charge integration) and discriminator of different shapes for particle identification. These special firmware make the digitizer an enhanced system for Physics Applications. 

Mod. DT5720是2/4通道12位250 MS/s桌面波形数字化仪，单端MCX同轴连接器上有2 Vpp输入动态范围(见表1.1)。直流偏移是可调节的，通过一个16位DAC在1 V范围内的每个通道。

考虑到采样频率和比特数，这些数字发生器非常适合于中快信号，如来自液体或无机闪烁体与PMTs或硅光电倍增管耦合的信号。

通过前面板TRG-IN输入连接器或软件，可以向外部提供一个通用的采集触发信号(对所有通道都是通用的)。或者，当输入信号低于/超过可编程阈值时，每个通道能够产生自触发器。一个板的触发器可以通过前面板GPO传播到板外。

在采集过程中，数据流被连续写入一个循环内存缓冲区。当触发器发生时，数字化仪将进一步写入后触发器的样本，并冻结可以被所提供的读取链接之一读取的缓冲区。

每个通道都有一个SRAM数字存储器(见表1.1的可用内存大小选项)，被分割成可编程大小的缓冲区(1/1024)。读取(从USB或光学链路)的冻结缓冲区是独立于写操作在活动循环缓冲区(ADC数据存储)。

单板存储器支持两种事件存储模式:标准模式和Pack2.5模式(参见Sec. event structure)。

DT5720具有前面板CLK-IN连接器以及内部锁相环用于内部/外部参考的时钟合成(50 MHz振荡器)。

单板提供usb2.0接口和光链路接口。usb2.0允许数据传输高达30 MB/s。光链路接口(CAEN专有的CONET协议)能够传输速率高达80mb /s，并提供菊花链能力。因此，最多可以连接8个ADC模块到一个A2818光链路控制器，或者使用4链路的A3818版本连接最多32个ADC模块(Mod. A2818/A3818，见表1.1)。

除了波形记录固件，CAEN为数字化转换器提供了用于脉冲形状识别(DPP- psd) [RD2]的数字脉冲处理固件(DPP)，该固件结合了数字QDC(电荷集成)的功能和用于粒子识别的不同形状的鉴别器。这些特殊的固件使数字化仪成为物理应用的增强系统。