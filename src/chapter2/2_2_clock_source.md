# 2.2 时钟源驱动
时钟是嵌入式系统的"心跳"，它驱动着系统的运行和数据的流动，同时影响着系统的能耗．因此，往往针对SoC中不同的外设，有多个不同频率的时钟源可供选择，以满足不同场景和能耗需求．外设的时钟以一种树状的形式在SoC中被组织起来，我们称之为"时钟树"．

在linux中，外设驱动对时钟的操作是由[Common Clk Framework](https://docs.kernel.org/driver-api/clk.html)所提供的．