#	 								Cortex-M4 TIM** **定时器中断**

##	1. 实验目的

熟悉 STM32CubeIDE 工具软件的使用。

掌握 STM32CubeIDE 软件的基本设计流程和设计步骤，能够使用工具进行设计、
编程、仿真调试。

学习通用定时器的使用方法，掌握如何利用 STM32MP157A 芯片的通用定时器定时产生中断。

## 2.实验环境

FS-MP1A 开发平台

ST-Link 仿真器

STM32CubeIDE 开发软件

PC 机 XP、Window7/10 (32/64bit)

## 3. 实验原理

STM32 系列微控制器具有多种定时器，其中包括基本定时器，通用定时器，高级定时器。

几种定时器功能比较：

1、基本定时器：主要运用于定时器计数及驱动 DAC

2、通用定时器：定时器定时计数、输入捕获、输出比较、PWM 输出、使用外部信号控制定时器和定时器互连的同步电路

3、高级定时器：通用定时器的所有功能、带死区控制和紧急刹车，可用于 PWM 控制电机

本章节实验以通用定时器 TIM3 为例实现定时计数，计数到设置值后触发中断改变 LED灯亮灭状态。

从下图可以看出定时器时钟 TIM3 挂载在 APB1 时钟总线上，在 STM32CubeIDE 软件中可配置总线时钟频率来确定定时器时钟。

![image-20240101203729796](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240101203729796.png)

![image-20240102211200972](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240102211200972.png)

[^基本定时器功能框图]: 

从上图我们可以看到，基本定时器主要由下面几部分组成

**时钟源**

定时器要实现计数必须有个时钟源，基本定时器时钟只能来自内部时钟，高级控制定时器和通用定时器还可以选择外部时钟源或者直接来自其他定时器等模式。

**控制器**

定时器控制器控制实现定时器功能，控制定时器复位、使能、计数是其基础功能，基本定时器还专门用于 DAC 转换触发。

**计数器**

基本定时器计数过程主要涉及到三个寄存器内容，分别是计数器寄存器(TIMx_CNT)、预分频器寄存器(TIMx_PSC)、自动重载寄存器(TIMx_ARR)，这三个寄存器都是 16 位有效数字，即可设置值为 0 至 65535。上图中预分频器 PSC，它有一个输入时钟 CK_PSC 和一个输出时钟 CK_CNT。输入时钟 CK_PSC 来源于控制器部分，基本定时器只有内部时钟源所以 CK_PSC 实际等于 CK_INT。在不同应用场所，经常需要不同的定时频率，通过设置预分频 器 PSC 的 值 可以非 常 方 便得 到 不同的 CK_CNT ，实 际计 算 为 ： fCK_CNT 等 于fCK_PSC/(PSC[15:0]+1)。下图中可看到将预分频器 PSC 的值从 1 改为 4 时计数器时钟变化过程。原来是 1 分频，CK_PSC 和 CK_CNT 频率相同。向 TIMx_PSC 寄存器写入新值时，并不会马上更新 CK_CNT 输出频率，而是等到更新事件发生时，把 TIMx_PSC 寄存器值更新到影子寄存器中，使其真正产生效果。更新为 4 分频后，在 CK_PSC 连续出现 4 个脉冲后CK_CNT 才产生一个脉冲。
![image-20240102205512528](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240102205512528.png)

[^基本定时器时钟源分频]: 

在定时器使能(CEN 置 1)时，计数器 COUNTER 根据 CK_CNT 频率向上计数，即每来一个 CK_CNT 脉冲，TIMx_CNT 值就加 1。当 TIMx_CNT 值与 TIMx_ARR 的设定值相等时就自动生成事件并 TIMx_CNT 自动清零，然后自动重新开始计数，如此重复以上过程。为此可见，我们只要设置 CK_PSC 和 TIMx_ARR 这两个寄存器的值就可以控制事件生成的时间，而我们一般的应用程序就是在事件生成的回调函数中运行的。在 TIMx_CNT 递增至与TIMx_ARR 值相等，我们叫做为定时器上溢。自动重载寄存器 TIMx_ARR 用来存放于计数器值比较的数值，如果两个数值相等就生成事件，将相关事件标志位置位，生成 DMA 和中断输出。TIMx_ARR 有影子寄存器，可以通过 TIMx_CR1 寄存器的 ARPE 位控制影子寄存器功能，如果 ARPE 位置 1，影子寄存器有效，只有在事件更新时才把 TIMx_ARR 值赋给影子寄存器。如果 ARPE 位为 0，修改 TIMx_ARR 值马上有效。定时器周期计算

经过上面分析，我们知道定时事件生成时间主要由 TIMx_PSC 和 TIMx_ARR 两个寄存器值决定，这个也就是定时器的周期。比如我们需要一个 1s 周期的定时器，具体这两个寄存器值该如何设置内。假设，我们先设置 TIMx_ARR 寄存器值为 9999，即当 TIMx_CNT 从0 开始计算，刚好等于 9999 时生成事件，总共计数 10000 次，那么如果此时时钟源周期为100us 即可得到刚好 1s 的定时周期。接下来问题就是设置 TIMx_PSC 寄存器值使得 CK_CNT 输出为 100us 周期(10000Hz)的时钟。预分频器的输入时钟 CK_PSC 为 64MHz，所以设置预分频器值为(6400-1)即可满足
中断时间：1/（TIMxCLK/(PSC+1)）* (ARR+1)

## 4.实验步骤

1.   ​	创建工程

2.   
     ![image-20240102211933976](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240102211933976.png)

3.   
     ![image-20240102212023631](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240102212023631.png)

     

4.   搜索框内搜索 LED 对应 GPIO 引脚 PZ5、PZ6、PZ7，左键点击设置为 GPIO_Output
     ![image-20240102212241266](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240102212241266.png)

5.   这里我们需要注意一下，和其他单片机不同，还需要继续设置“ Pin Reservation”给

     “ Cortex-M4”，否则 STM32CubeMX 不会生生成 GPIO 初始化相关代码。具体操作：在刚才

     选择的引脚上，鼠标右键选择“ Pin Reservation”->“ Cortex-M4”。
     ![image-20240102212315918](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240102212315918.png)

6.   在 Code Generator 处选择为每个外设生成单独的 C 和 H 文件，这样设置方便阅读代码

7.   完成以上设置后，Ctrl+S 保存，会提示是否需要生成代码，选择 Yes 即可自动生成代码。

     系统会自动生成 System Clock 代码

8.   

     ```c
     /**
      *
      */
     void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
     {
     	if(htim->Instance == TIM3)
     	{
     		HAL_GPIO_TogglePin(GPIOZ,GPIO_PIN_5);
     	}
     
     }
     ```

9.   
     ![image-20240102220516542](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240102220516542.png)



##	5.实验结果

可看到 LED 灯间隔 1S 改变一次亮灭状态

