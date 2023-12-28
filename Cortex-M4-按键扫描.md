#							Cortex-M4-按键扫描

## 1. 实验目的

熟悉 STM32CubeIDE 工具软件的使用。

掌握 STM32CubeIDE 软件的基本设计流程和设计步骤，能够使用工具进行设计、编程、

仿真调试。

学习 GPIO 口的使用方法，掌握如何利用 STM32MP157A 芯片的 I/O 口读取按键状态。

##	2. 实验环境

FS-MP1A 开发平台

ST-Link 仿真器

STM32CubeIDE 开发软件

PC 机 XP、Window7/10 (32/64bit)

##	3. 实验原理

本实验主要通过调用函数 HAL_GPIO_ReadPin 读取开发板上按键 K2 的 IO 口状态，控制LED 灯的点亮和关闭状态。

该实验需要使用到开发板上的 LED 灯，按键，相关硬件电路如下：
![image-20231228214531266](.\Picture\image-20231228214531266.png)

## 4. 实验步骤

1.   工程创建依照STM32CubeLED环境安装的 1-7步骤

2.   搜索框内搜索 LED 对应 GPIO 引脚 PZ5，左键点击设置为 GPIO_output

     搜索框内搜索 K2 对应 GPIO 引脚 PA0，左键点击设置为 GPIO_Input
     ![image-20231228215634046](.\Picture\image-20231228215634046.png)

3.   这里我们需要注意一下，和其他单片机不同，还需要继续设置“ Pin Reservation”给

     “ Cortex-M4”，否则 STM32CubeMX 不会生生成 GPIO 初始化相关代码。具体操作：在刚

     才选择的引脚上，鼠标右键选择“ Pin Reservation”->“ Cortex-M4”。
     ![image-20231228215704028](.\Picture\image-20231228215704028.png)

4.   在 Code Generator 处选择为每个外设生成单独的 C 和 H 文件，这样设置方便阅读代码
     ![image-20231228215741239](.\Picture\image-20231228215741239.png)

​			完成以上设置后，Ctrl+S 保存，会提示是否需要生成代码，选择 Yes 即可自动生成代码。系统会自动生成 System Clock 代码

![image-20231228215819857](.\Picture\image-20231228215819857.png)

5.   在 main.c 中调 while 循环中添加 GPIO 读函数，判断 K2 引脚高低电平，改变 LED 灯状态

     ![image-20231228223946248](.\Picture\image-20231228223946248.png)

## 5. 实验现象

 按下 K2 键，可看到 LED 灯状态发生改变
