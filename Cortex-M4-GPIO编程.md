#										Cortex-M4 GPIO编程

##		1.实验目的

​	熟悉 STM32CubeIDE 工具软件的使用。

​	掌握 STM32CubeIDE 软件的基本设计流程和设计步骤，能够使用工具进行设计、编程、

​	仿真调试。

​	学习 GPIO 口的使用方法，掌握如何利用 STM32MP157A 芯片的 I/O 口控制 LED。

##	2.实验环境

​	FS-MP1A 开发平台

​	ST-Link 仿真器

​	STM32CubeIDE 开发软件

​	PC 机 XP、Window7/10 (32/64bit)

##	3.实验原理

只要是对硬件操作，就要首先查看原理图。查看外设是和模块的 MCU 的哪个引脚相连。

FS-MP1A 开发平台上的 LED 的亮灭状态，与芯片上的引脚 I/O 输出电平有关。

FS-MP1A 开发平台上 LED 的 I/O：
![image-20231226225208309](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231226225208309.png)

IO 操作重要结构体：GPIO_InitTypeDef

```c
/**
  * @brief   GPIO Init structure definition
  */
typedef struct
{
  uint32_t Pin;       /*!< Specifies the GPIO pins to be configured.
                           This parameter can be any value of @ref GPIO_pins_define */

  uint32_t Mode;      /*!< Specifies the operating mode for the selected pins.
                           This parameter can be a value of @ref GPIO_mode_define */

  uint32_t Pull;      /*!< Specifies the Pull-up or Pull-Down activation for the selected pins.
                           This parameter can be a value of @ref GPIO_pull_define */

  uint32_t Speed;     /*!< Specifies the speed for the selected pins.
                           This parameter can be a value of @ref GPIO_speed_define */

  uint32_t Alternate;  /*!< Peripheral to be connected to the selected pins.
                            This parameter can be a value of @ref GPIO_Alternate_function_selection */
}GPIO_InitTypeDef;
```

第一个成员变量 Pin 是所操作的管脚
第二个 Mode 是模式选择
第三个 Pull 是上拉下拉，或者都不加
第四个 Speed 是速度选择，第五个是管脚复用功能。
一般我们只操作前四个。
IO 口可以由软件配置成 4 种模式，其实操作的是 GPIO 的端口模式寄存器：
	输入（复位状态）/input（reset state）
	通用输出模式 / general purpose output mode
	复用功能模式 / alternate function mode
	模拟模式 / analog mode

```c
/** @defgroup GPIO_mode_define  GPIO mode define
  * @brief GPIO Configuration Mode
  *        Elements values convention: 0xX0yz00YZ
  *           - X  : GPIO mode or EXTI Mode
  *           - y  : External IT or Event trigger detection
  *           - z  : IO configuration on External IT or Event
  *           - Y  : Output type (Push Pull or Open Drain)
  *           - Z  : IO Direction mode (Input, Output, Alternate or Analog)
  * @{
  */
#define  GPIO_MODE_INPUT                        ((uint32_t)0x00000000U)   /*!< Input Floating Mode                   */
#define  GPIO_MODE_OUTPUT_PP                    ((uint32_t)0x00000001U)   /*!< Output Push Pull Mode                 */
#define  GPIO_MODE_OUTPUT_OD                    ((uint32_t)0x00000011U)   /*!< Output Open Drain Mode                */
#define  GPIO_MODE_AF_PP                        ((uint32_t)0x00000002U)   /*!< Alternate Function Push Pull Mode     */
#define  GPIO_MODE_AF_OD                        ((uint32_t)0x00000012U)   /*!< Alternate Function Open Drain Mode    */

```

上面给两个寄存器赋值了，1~4 位是 GPIO 端口模式寄存器，5~8 位是端口输出类型寄存器（决定是推挽输出还是开漏输出）。

STM32 的 GPIO 端口在作为输出时，可以软件配置端口最大支持的时钟速率，下图是端口输出速度寄存器，有以下几种速度选择：
![image-20231226230116224](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231226230116224.png)

```c
/**
  * @}
  */

/** @defgroup GPIO_speed_define  GPIO speed define
  * @brief GPIO Output Maximum frequency
  * @{
  */
#define  GPIO_SPEED_FREQ_LOW         ((uint32_t)0x00000000U)  /*!< Low speed     */
#define  GPIO_SPEED_FREQ_MEDIUM      ((uint32_t)0x00000001U)  /*!< Medium speed  */
#define  GPIO_SPEED_FREQ_HIGH        ((uint32_t)0x00000002U)  /*!< Fast speed    */
#define  GPIO_SPEED_FREQ_VERY_HIGH   ((uint32_t)0x00000003U)  /*!< High speed    */
/**
  * @}
  */
```

GPIO 调用的 HAL 函数：

该函数其实是对 BSRR 寄存器进行操作。

```c
void HAL_GPIO_WritePin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin, GPIO_PinState PinState);
```

第一个参数传的是 GPIO 所在的组，第二个是该组的几号管脚，第三个是对管脚进行置位。

## 4.实验步骤

 1.    工程创建依照STM32CubeLED环境安装的 1-7步骤

 2.    在第7步骤对GPIO引脚进行输出设置的时候,
       这里我们需要注意一下，和其他单片机不同，还需要继续设置“ Pin Reservation”给

       “ Cortex-M4”，否则 STM32CubeMX 不会生生成 GPIO 初始化相关代码。具体操作：在刚才

       选择的引脚上，鼠标右键选择“ Pin Reservation”->“ Cortex-M4”。
       ![image-20231227223031781](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231227223031781.png)

3.   其余生成步骤一样.

4.   添加 GPIO 函数说明：

     我们需要在 main.c 中添加 GPIO 相关函数， GPIO 引脚输出电平高低函数
     ![image-20231227223346865](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231227223346865.png)

5.   在 main.c 中调用函数改变 GPIO 引脚高低电平来改变 LED 灯的状态
     ![image-20231227224233400](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231227224233400.png)

## 	4.实验现象

可看到 LED 灯循环亮灭

![image-20231227224503063](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231227224503063.png)
