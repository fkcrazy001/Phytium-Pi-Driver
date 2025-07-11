# 3.3 SPI驱动开发
## SPI 基础 ##
SPI(rial Peripheral interface)串行外围设备接口,从名称上就可以明白这个总线使用的还是串行通信，SPI一种主从模式**高速**的**全双工**串行总线。
SPI接口一般使用四条信号线通信：MISO（数据输入），MOSI（数据输出），SCK（时钟），CS（片选）  
**MISO**：主设备输入/从设备输出引脚。该引脚在从模式下发送数据，在主模式下接收数据。  
**MOSI**：主设备输出/从设备输入引脚。该引脚在主模式下发送数据，在从模式下接收数据。  
**SCLK**：串行时钟信号，由主设备产生。  
**CS/SS**：从设备片选信号，由主设备控制。它的功能是用来作为“片选引脚”，也就是选择指定的从设备，让主设备可以单独地与特定从设备通讯，避免数据线上的冲突。
![单个spi设备连接](../resource/img/3_3_1_spi_one_link.webp)
![多个spi设备连接](../resource/img/3_3_2_spi_many_link.webp)

常见的连接方法有上面两种，还有一种额外电路所能实现的片选方案。众所周知三个引脚其实是存在8种状态，所以实际上可以利用译码器实现三个CS/SS控制8个SPI设备。
![3-8译码器](../resource/img/3_3_4_3-8译码器.png)

SPI 与一些常见的串行总线相比，有以下三个显著的区别：

**① 无传输速率限定**  
SPI 没有严格的速度限制，一般实现通常能达到甚至超过 **100Mbps**，适合高速数据传输场景。

**② 传输方式为数据交换**  
SPI 工作在主从模式下，但在每个时钟周期（Clock）内，主从设备都会**同时发送和接收 1 bit 数据**，实现真正的**全双工数据交换**。无论是主设备还是从设备，每个时钟周期都会有数据交换。
只不过是主机进行写操作时，主机只需忽略接收到的字节；反之，若主机要读取从机的一个字节，就必须发送一个空字节来引发从机的传输。
![传输模型](../resource/img/3_3_3_spi_transfer_mode.webp)
**③ 没有明确的协议格式**  
SPI 没有指定的流控制，没有应答机制确认是否接收到数据，只要四根信号线连接正确，SPI模式相同，将CS/SS信号线拉低，即可以直接通信，一次一个字节的传输，读写数据同时操作。SPI并不关心物理接口的电气特性，例如信号的标准电压

## 飞腾派上SPI实验 #