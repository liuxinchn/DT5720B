# 720 DPP-PSD Register Description and Data Format



## Purpose of this Manual

用户手册包含720家族系列的DPP-PSD固件寄存器的完整描述。符合DPP-PSD固件版本4.17 _131.11。



## Symbols, abbreviated terms and notation 

ADC     				
AMC 					
DAQ 				
DAC 
DC 
DPP 
DPP-QDC 
DPP-PHA 
DPP-PSD 
LVDS 
ROC 
USB 



## Registers and Data Format

用户手册中描述的所有寄存器都是32位宽的。在vme访问的情况下，可以使用A24和A32寻址模式。



### Register Address Map

下表报告了用户可以访问的寄存器的完整列表。可以单击第一列中的寄存器名称，将其重定向到相关的寄存器描述。寄存器地址在第二列中以十六进制值的形式报告。第三列表示允许的寄存器访问模式

- I : Individual register。这种寄存器有N个实例，其中N是板中通道的总数。单个寄存器可以以单一模式(单个设置)或广播(对所有通道的同时写访问)写入。读命令必须是个别的。单次访问可以在地址0xlnXY执行，其中n是通道号，而广播写可以在地址0x80XY执行
- C: common Register。具有此属性的寄存器只有一个实例，因此只能在地址0x80XY上执行读和写访问。



### Short Gate Width 

为脉冲形状鉴别中快速元件的电荷积分设置短栅宽度.



### Long Gate Width

为脉冲形状鉴别中慢速元件的电荷积分设置长栅宽度。用长积分门进行能谱计算



### Gate Offset 

为了正确地集成输入脉冲，集成门在触发器位置之前启动。Gate Offset定义了Gate在触发器之前启动多少个样本。



### Trigger Threshold 

设置前沿识别的触发阈值



### Fixed Baseline

基线计算可以动态或静态地执行。在第一种情况下，用户可以通过寄存器0xln80设置移动平均窗口的样本。在后一种情况下，用户必须禁用通过寄存器0xln80的比特[22:20]自动基线计算，并通过该寄存器设置所需的附加基线值。基线值在整个采集过程中保持不变。



### Trigger Latency

该寄存器允许用户设置一个时间窗口(延迟)，添加到形状触发器宽度(即相关窗口的长度)中，当设置数字化器通道之间的重合/反重合时，需要考虑从背带到主板的触发器传播中的延迟。



### Shaped Trigger Width 

形状触发器是一个可编程宽度的逻辑信号，由通道生成，对应于其本地自触发器(即前缘鉴别器的输出)。它用于将触发器传播到板的其他通道和其他外部板，以及提供符合触发器逻辑。



### Threshold for the PSD cut 

设置PSD阈值，根据事件的PSD值在线选择事件。PSD范围为0到1。



### PUR-GAP Threshold 

当同一门内出现“峰-谷-峰”情况时，检测到堆积事件。谷和峰之间的间隙可以通过这个寄存器来编程。更多细节请参考CoMPASS用户手册。



### DPP Algorithm Control 

管理DPP算法的特点。

**从0~31字节，各自有不同过的功能。**



### Channel n Status

该寄存器包含通道n的状态信息。



### AMC Firmware Revision 

返回固件的版本



### DC Offset 

该寄存器允许调整ADC刻度上输入信号的基线位置(即0伏)。ADC的规模范围从0到  2<sup>NBit</sup>  -1，其中NBit是板载ADC的位数。控制直流偏移量的DAC有16位，即它从0到65535独立于NBit值和单板类型。

通常DC Offset值为32K (DAC中等刻度)对应ADC中等刻度。直流偏移量的增大使基线减小。DAC的范围大约比ADC的范围大5%(典型)，因此接近O和64K的DAC设置分别对应ADC的超出和低于范围。

警告:在写入此寄存器之前，必须检查Oxln88位[2]= 0，否则写入进程将无法正常运行!



### Board Configuration

该寄存器包含单板配置的一般设置。

**从0~31字节，各自有不同过的功能。**



### Aggregate Organization 总组织

数字化器的内部存储器可以划分为可编程数量的聚合，其中每个聚合包含特定数量的事件。这个寄存器定义了内存中可以包含多少个聚合。

注意：在采集过程中，这个寄存器不能被修改。



### Record Length 

设置波形采集的记录长度 



### Number of Events per Aggregate 每个聚合的事件数

每个通道都有固定数量的RAM内存来保存事件。内存被划分为可编程数量的缓冲区，称为“聚合”，其事件数量可以通过这个寄存器编程。



### Pre Trigger 

预触发器定义了保存到内存中的波形中触发器之前的采样数量。



### Trigger Hold-Off Width 触发延迟宽度

触发延迟是一种可编程宽度的逻辑信号，由信道产生，与它的本地自触发相对应。其他触发器在整个触发器暂停持续时间内被抑制



### Acquisition Control 

该寄存器管理采集设置。

**从0~31字节，各自有不同过的功能。**



### Acquisition Status 

该寄存器监视与获取状态相关的一组条件。

**从0~31字节，各自有不同过的功能。**



### Software Trigger 

写入这个寄存器会生成一个软件触发器，该触发器被传播到板上所有启用的通道。



### Global Trigger Mask 

该寄存器设置哪些信号可用于生成全局触发器。

**从0~31字节，各自有不同过的功能。**



### Front Panel TRG-OUT (GPO) Enable Mask 

这个寄存器设置了哪些信号可以在前面板TRG-OUT LEMO连接器上产生信号(在DT和NIM板情况下为GPO)。

**从0~31字节，各自有不同过的功能。**



### Front Panel I/O Control 

这个寄存器管理前面板I/O连接器。缺省值为0x000000。

**从0~31字节，各自有不同过的功能。**



### Channel Enable Mask

该寄存器启用/禁用所选通道参与事件读取。禁用的通道不可用。

警告:此寄存器不能在采集运行时被修改。



### ROC FPGA Firmware Revision 

该寄存器包含主板FPGA (ROC)固件修订信息。



### Voltage Level Mode Configuration 

当电压水平模式被启用(位[2:0]= 100 (bin)的寄存器0x8144)，该寄存器设置DAC值提供在前面板MON/Sigma输出LEMO连接器:1 LSB = 0.244 mV，终止在50欧姆。

注意:此寄存器仅由VME板支持。



### Software Clock Sync 

上电时，固件向adc发出Sync命令，使所有adc与板上时钟同步。在标准操作中，该命令不需要用户重复执行。

对该寄存器的写访问(任意值)迫使锁相环重新将所有时钟输出与参考时钟对齐。

示例:在VME板之间分布菊花链时钟的情况下，在初始化和配置过程中，菊花链上的参考时钟可能不稳定，锁相环中可能出现暂时的失锁;尽管一旦参考时钟恢复稳定，锁就会自动恢复，但不能保证相移返回到已知状态。该命令允许单板恢复CLK-IN和内部时钟之间的正确相移。

注意:此寄存器仅由VME板支持。

注意:该命令必须从时钟链的第一个板到最后一个板开始下发。



### Board Info 

该寄存器包含电路板的特定信息，如数字化器系列、通道内存大小和通道密度。



### Analog Monitor Mode 

该寄存器选择在MON/Sigma前面板LEMO连接器上提供哪种输出模式。

注意:此寄存器仅由VME板支持。



### Event Size 

该寄存器包含当前事件大小，用32位字表示。该值在每个事件完全读出后更新。



### Time Bomb Downcounter 倒计时

这是一个向下的计数器值。如果为固定值，则启用固件license，可以不受时间限制地使用当前的固件。如果该值随时间递减，则模块上电30分钟后固件将停止工作(无法进入RUN模式)。如果该值为0，表示该时间炸弹已过期，在不上电的情况下，不允许模块以RUN模式进入。



### Fan Speed Control 

该寄存器管理板上风扇转速，以保证根据内部温度变化进行适当的冷却。

注意:从主板PCB的修订版4(参见配置ROM的寄存器0xF04C)，自动风扇速度控制已经实现，并由大于4.4的ROC FPGA固件修订版支持(参见寄存器0x8124)。

独立的修订，用户可以设置位[3]= 1风扇转速高。设置位[3]= 0将恢复自动控制的修订版4或更高，或低风扇转速的情况下修订版4。

注意:此寄存器仅由桌面(DT)板支持。



### Run/Start/Stop Delay 

当运行的开始被同步地给予在菊花链中连接的几个板时，有必要补偿开始(或停止)信号通过链条传播的延迟。这个寄存器设置了在板的输入端(S-IN/GPI或TRG- IN上)到达开始信号和实际开始运行之间的延迟。延迟通常为零在链的最后一个板和上升沿链。



### Board Failure Status

这个寄存器监视一组板错误。如果发生故障，则在数据读取期间，将事件格式头的第二个单词中的位[26]设置为1(有关事件结构的描述，请参阅digitizer用户手册)。读取该寄存器可以检查发生了哪种错误。

注意:如果单板出现问题，建议用户联系CAEN寻求支持。



### Disable External Trigger 

可以通过这个寄存器禁用TRG-IN连接器上的外部触发器。任何与TRG-IN相关的功能也被禁用。



### Trigger Validation Mask

设置触发器验证逻辑



### Front Panel LVDS I/O New Features 

如果启用 LVDS I/O 新特性(位[8]= 0x811C的1)，该寄存器编程前面板LVDS I/O 16针连接器的功能。可以通过四组(4)配置LVDS I/O引脚。

选项是:1)0000 = REGISTER，其中四个LVDS I/O引脚作为寄存器(根据配置的输入/输出选项读/写);2) 0001 = TRIGGER，其中每组四个LVDS I/O引脚可以配置为接收每个通道的输入触发器(仅DPP固件)，或传播触发器请求;3) 0010 = nBUSY/nVETO，其中每组4个LVDS I/O引脚可以配置为输入(0 = nBusyln, 1 = nVetoln, 2 = nTrigger In, 3 = nRun In)或输出(0 = nBUSY, 1 = nVETO, 2 = nTrigger Out, 3 = nRun);4) 0011 = LEGACY，即根据旧的LVDS I/O配置(即ROC FPGA固件版本低于3.8)，其中LVDS在输入LVDS设置时可以配置为0 = nclear TTT, 1 = 2 = 3 =保留，而在输出LVDS设置时可以配置为0 = Busy, 1 = Data ready, 2 = Trigger, 3 = Run。

详细描述请参考数字化仪用户手册的前面板LVDS I/O部分。

注意:LVDS I/O新功能支持从ROC FPGA固件版本3.8上。

注意:此寄存器仅由VME板支持。



### Buffer Occupancy Gain

如果选择缓冲占用模式(位[2:0]= 0x8144的011)，LEMO MON/Sigma输出连接器提供一个电压水平，其振幅以固定步长增加，恰好与事件缓冲区中的事件数量一致。每步输出电压水平为0.976 mV。增益可以通过这个寄存器加到阶跃上。允许的取值范围为[0:A]。默认值0表示没有增益，而写入0xn则表示固定步长为0.976*2n mV。

注意:此寄存器从ROC FPGA固件4.9版本开始支持。

注意:此寄存器仅由VME板支持。



### Extended Veto Delay

该寄存器仅对VME板有效，当0x8100寄存器的位[12]=1时，在TRGOUT上设置触发抑制的扩展Vetoln信号的持续时间。这种函数在多板系统同步的特殊情况下是有用的。

注意:此寄存器从ROC FPGA版本4.16起有效。



### Readout Control 

该寄存器主要用于VME板，无论如何，一些位也适用于DT和NIM板。



### Readout Status 

该寄存器包含与读出相关的信息。



### Board ID 

这个寄存器的意义取决于它被插入到哪个VME板条箱中。

在VME64X板条箱版本的情况下，该寄存器只能在读模式下访问，它包含从背板连接器中选取的模块的GEO地址;当执行CBLT时，GEO地址将包含在事件头的Board ID字段中(详细信息请参见用户手册)。

在其他板条箱版本的情况下，该寄存器可以在读写模式下访问，它允许在CBLT操作之前写入模块的正确GEO地址(默认设置= O)。GEO地址将包含在事件报头的Board ID字段中(详细信息请参见用户手册)。

注意:此寄存器仅由VME板支持。



### MCST Base Address and Control 

该寄存器为VME组播周期配置单板。

注意:此寄存器仅由VME板支持。



### Relocation Address

如果通过寄存器0xEF00(位[6]= 1)启用地址重定位，该寄存器设置模块的VME基址。

注意:此寄存器仅由VME板支持。



### Interrupt Status/ID 

这个寄存器包含了模块在中断确认周期期间放在VME数据总线上的STATUS/ID。

62注意:此寄存器仅由VME板支持。



### Interrupt Event Number

这个寄存器设置了导致中断请求的事件数量。如果中断被启用，模块将在内存中存储一个事件数目 > 中断事件数目时生成一个请求。



### Aggregate Number per BLT 

该寄存器设置了每个块传输(通过VME BLT/CBLT循环或通过光链路读取块)必须传输的完整聚合的最大数量。



### Scratch 

这个寄存器可以用来写/读用于测试的单词。



### Software Reset 软件复位

所有的数字化器寄存器都可以在软件复位命令上通过在这个寄存器上写入任何值，或者在VME板的情况下通过从背板上的系统复位，将其设置回默认值



### Software Clear 

所有的数字化器内部内存被清除:

- 自动由固件在每次运行开始;
- 在此寄存器写入软件命令;
- 通过硬件(仅VME板)通过LVDS接口正确配置。

clear命令不会改变寄存器的实际值，除非重置以下寄存器:

- 事件存储

- 事件大小;
- 通道/组n缓冲占用率。

该寄存器还重置触发器时间戳。



### Configuration Reload 

在这个位置的任何值的写访问都会导致软件重置，重新加载配置ROM参数和锁相环重新配置。



### Configuration ROM Checksum 

该寄存器包含配置ROM空间的8位校验和信息。



### Configuration ROM Checksum Length BYTE 2 

这个寄存器包含3字节校验和长度的第三个字节的信息(即配置ROM到校验和的字节数)。



### Configuration ROM Checksum Length BYTE 1 

这个寄存器包含3字节校验和长度的第二个字节的信息(即配置ROM到校验和的字节数)。



### Configuration ROM Checksum Length BYTE 0 

这个寄存器包含3字节校验和长度的第一个字节的信息(即配置ROM到校验和的字节数)。



### Configuration ROM Constant BYTE 2 

这个寄存器包含3字节常量的第三个字节。



### Configuration ROM Constant BYTE 1 

该寄存器包含3字节常量的第二个字节。



### Configuration ROM Constant BYTE 0

该寄存器包含3字节常量的第一个字节。



### Configuration ROM C Code 

该寄存器包含ASCII C字符代码(将其标识为CR空格)。



### Configuration ROM R Code

该寄存器包含ASCII R字符代码(将其标识为CR空格)。



### Configuration ROM IEEE OUI BYTE 2 

该寄存器包含3字节IEEE组织唯一标识符(OUI)的第三字节信息。



### Configuration ROM IEEE OUI BYTE 1 

该寄存器包含3字节IEEE组织唯一标识符(OUI)的第二个字节上的信息。



### Configuration ROM IEEE OUI BYTE 0 

该寄存器包含3字节IEEE组织唯一标识符(OUI)的第一个字节上的信息。



### Configuration ROM Board Version 

该寄存器包含单板版本信息。



### Configuration ROM Board Form Factor 

该寄存器包含电路板形状因子的信息。



### Configuration ROM Board ID BYTE 1 

该寄存器包含2字节板标识符的MSB



### Configuration ROM Board ID BYTE 0 

该寄存器包含2字节板标识符的LSB信息。



### Configuration ROM PCB Revision BYTE 3 

该寄存器包含关于4字节硬件修订版的第4字节的信息。



### Configuration ROM PCB Revision BYTE 2 

该寄存器包含关于4字节硬件版本的第三个字节的信息。



### Configuration ROM PCB Revision BYTE 1 

该寄存器包含关于4字节硬件版本的第二个字节的信息。



### Configuration ROM PCB Revision BYTE 0

该寄存器包含关于4字节硬件版本的第一个字节的信息。



### Configuration ROM FLASH Type 

该寄存器包含板上FLASH类型(存储FPGA固件)的信息。



### Configuration ROM Board Serial Number BYTE 1 

This register contains information on the MSB of the board serial number. 



### Configuration ROM Board Serial Number BYTE 0 

该寄存器包含单板序列号的LSB信息。



### Configuration ROM VCXO Type 

该寄存器包含关于机载VCXO类型的信息。





## DPP-PSD Memory Organization 

每个通道都有固定数量的RAM内存来保存事件。内存被划分为可编程数量的缓冲区(也称为“聚合”)，其中每个缓冲区包含可编程数量的事件。事件格式也是可编程的。

- “聚合组织”(Nb)，地址0x800C:定义内存中可以包含多少聚合(naggr = 2Nb)。
- “每聚合事件数量”(Ne)，地址0xln34:定义一个聚合中包含的事件数量。允许的最大值为1023。
- “记录长度”(Ns)，地址0xln20:定义波形采集的采样数量，当启用时(rec_len = Ns * 8)。
- “Board Configuration”，地址0x8000:定义了采集方式和事件数据格式。

注意:那些需要编写自己的DAQ软件的人，必须注意根据事件和缓冲区大小选择Ne值，在下一节的示例中解释。

关于在CAENDigitizer库中使用这些参数的信息可以在[RD1]中找到。根据编程的事件格式，一个事件可以包含一定数量的波形样本，一个触发时间戳，两个电荷Cl。短和长，和Extras信息。



### 720 series 

以下部分描述720系列的内存组织结构。板内的物理内存由内存位置组成，每个位置都是128位(16B)。根据地点占用率:

- 触发时间戳= 1 locatiion;
- 波形(如果启用)=1 location every 8 samples;
- 电荷(QL和QS)和EXTRAS=  1 locatiion。

图2.1显示了保存到720系列物理内存中的数据格式。

注:图2.1是指将事件存储到板的物理内存中。然后用不同的格式组织数据，以便事件读取。事件读出格式在事件数据格式一节中显示。

如前所述，“记录长度”和“单板配置”设置决定了事件的大小;用户必须计算每个缓冲区的事件数量(Ne)和相应的缓冲区数量(2Nb)。当板在列表模式下运行时，事件内存只包含两个位置，一个用于触发时间标签，另一个用于充电和基线。因此它非常小，建议使用较大的Ne值使缓冲区大小至少达到几KB。较小的缓冲区大小导致较低的读出带宽。为Ne设置高值的唯一缺点是，直到缓冲区完成，事件才可用于读取;因此，在触发器到达和读出相关事件数据之间存在一定的延迟。相反，当板运行在示波器模式，特别是当记录长度很大时，它更方便保持Ne低(通常是1)。

示例:假设混合模式被启用，Ns被设置为400个样本:事件大小(在位置)= 1{Time_Stamp) + Ns/8(波形)+ 1{Charge_EXTRA) = 52 loc。假设设置Ne= 60(每个缓冲区的事件数)，因此:buffer_size (in locations)= 52 * 60 = 3120 loc。假设内存板是128k loc./ch时，缓冲区的数量为:128k/3120 = 42(缓冲区)。该值对应于内存所能包含的缓冲区的最大数量。然而，由于可编程值必须是2的幂，用户必须选择小于42的最接近的数字，可以表示为2的幂，即2<sup>5</sup> = 32(即Nb= 5必须写入“聚合组织”寄存器)。

Example2:假设混合模式被启用，Ns被设置为24个样本:事件大小(在位置)= 1{Time_Stamp) + Ns/8(波形)+ 1{Charge_EXTRAS) = 5 loc。由于事件大小较小，将内存划分为几个更大的缓冲区来存储大量事件是很方便的。假设设Nb= 3，那么缓冲的个数是8。假设板内存选项由64k个位置组成，每个缓冲区由64k/8 = 8k个位置组成，因此每个聚合产生的事件数应为:Ne= 8k/5 = 1639。重要提示:在本例中，由于前面提到的寄存器长度限制，每个聚合存储的事件的实际数量是1023。



### Event Data Format

固件提供的数据格式被分组到事件聚合中。然后将每个通道聚合分组到板聚合中，最后分组到块传输中。那些需要自己编写采集软件的人必须注意以下部分。



#### Channel Aggregate Data Format for 720 series 

通道聚合由一组Ne事件组成，其中Ne是一个聚合中包含的可编程事件数(参见前一节)。720系列的两个事件(EVENT 0和EVENT 1)的Channel Aggregate结构如图2.2所示



#### Board Aggregate Data Format 

对于每个读取请求(发生在至少一个通道有可用数据可读取时)，“接口FPGA (ROC)”从每个启用的通道内存中读取一个聚合。每个通道每次读取的聚合不超过一个。通道聚合的样本是板聚合。如果一个通道没有数据，该通道就不会进入Board Aggregate。当VME的8个通道都有可用数据时，720系列的数据格式如图2.3所示:



#### Data Block 

数字化器的读取使用块传输(BLT，参见[RDl])完成;对于每次传输，板给出一定数量的板聚合，构成数据块。BLT中可以传输的最大聚合数由每个BLT的聚合数定义。在最后的读数中，每个板集合依次出现。如果有n个板集料，则数据块如图2.4所示