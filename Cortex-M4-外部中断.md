#												Cortex-M4-外部中断	

##	1.实验目的

熟悉 STM32CubeIDE 工具软件的使用。

掌握 STM32CubeIDE 软件的基本设计流程和设计步骤，能够使用工具进行设计、编程、仿真调试。

学习 GPIO 口的使用方法，掌握如何利用 STM32MP157A 芯片的 I/O 口作为外部中断输入。

##	2.实验环境

FS-MP1A 开发平台

ST-Link 仿真器

STM32CubeIDE 开发软件

PC 机 XP、Window7/10 (32/64bit)

##	3.实验原理

本实验的原理主要是，通过开发板上按键 K2 的外部中断方式读取键值即 IO 口状态，控制 LED 点亮、熄灭查看实验现象。

![image-20231230152329592](.\Picture\image-20231230152329592.png)

配置 IO 口外部中断的步骤：

1.   使 能 IO 口 时 钟 。

2.   初始化 IO 口 模 式 ， 触 发 条 件 。

3.   配置中断优先级（ NVIC ） ， 并 使 能 中 断 。

4.   在 中 断 服 务 函 数 中 调 用 外 部 中 断 共 用 入 口 函 数 HAL_GPIO_EXTI_IRQHandler 。

5.   编写外部中断回调函数
6.   通过以上几个步骤的设置，我们就可以正常使用外部中断了。

## 4.实验步骤

1.   工程创建依照STM32CubeLED环境安装的 1-7步骤.

2.   搜索框内搜索 LED 对应 GPIO 引脚 PZ5，左键点击设置为 GPIO_output

     搜索框内搜索 K2 对应 GPIO 引脚 PA0，左键点击设置为 GPIO_EXTI0	
     ![image-20231230152910747](.\Picture\image-20231230152910747.png)

3.   这里我们需要注意一下，和其他单片机不同，还需要继续设置“ Pin Reservation”给“Cortex-M4”，否则 STM32CubeMX 不会生生成 GPIO 初始化相关代码。具体操作：在刚才选择的引脚上，鼠标右键选择“ Pin Reservation”->“ Cortex-M4”
     ![image-20231228215704028](.\Picture\image-20231228215704028.png)
     
4.   修改位置

     ![image-20231230170736909](.\Picture\image-20231230170736909.png)

##	5.实验现象



