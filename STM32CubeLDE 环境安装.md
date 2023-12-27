#											STM32CubeLDE 环境安装

##	1. Cortex-M4开发环境安装

1.   STM32CubeIDE 是一个高级 C / C ++开发平台，具有用于 STM32 微控制器和微处理器的

     外设配置，代码生成，代码编译和调试功能。它基于 ECLIPSE™/ CDT 框架和用于开发的

     GCC 工具链，以及用于调试的 GDB。它允许集成数百个现有插件，这些插件可以完善

     ECLIPSE™IDE 的功能。

     **主要特点：**

     ⚫ 集成 STM32CubeMX，可提供以下服务：

     STM32 微控制器和微处理器的选择

     引脚排列，时钟，外设和中间件配置

     项目创建和初始化代码的生成

     ⚫ 基于 Eclipse™/ CDT，以支持 Eclipse™的附加软件，GNU C / C ++为 ARM ®工具链和

     GDB 调试器

     ⚫ 其他高级调试功能包括：

     CPU 内核，外设寄存器和内存视图

     实时变量观看视图

     系统分析和实时跟踪（SWV）

     CPU 故障分析工具

     ⚫ 支持 ST-LINK（STMicroelectronics）和 J-Link（SEGGER）调试探针

     ⚫ 从 Atollic 导入项目® TrueSTUDIO ®和 AC6 系统工作台的 STM32（SW4STM32）

     ⚫ 多操作系统支持：Windows ®，Linux 的®和 MacOS ®，仅 64 位版本

2.   **STM32CubeIDE** **软件获取**

     https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-ides/stm32cubeide.html?dl=C1Xl1v%2FULQ0dLNXhPUM9vg%3D%3D%2CcrEvVZcaQVFOpw%2BiBXN5OniVodWnWIjZczA3UQRxaf4t4IqBoA4ynj7KVdsgDWQkzo61Vd7IFSR48HcBaQB0YRJ%2BkmDJE3FQokO6ZkDEdtElQtf%2F5O4k7L07zP26x1rzWypenTcR3xmxW8zQdk0cZSrFINp1v0rofptktxG1ZiaKxBEZk%2BfFyQsFB9NEmpcO3%2Fali0QZwOaXOvoFFoLamIQEGD0T1zvkxSvhtnVFDrXK1rl7wv7VhFB0FbFa8NwpLv9lNr8K2z%2F9hdTjaL5tTN%2FRTqQyx%2FVXZA1UBill%2FgmJ7GsUcR3KHgrSsnv2S3bvl%2FddwcRJzcECHruxxQm562Y87gHg8Ff7A5ybUDdZAnFKsshPEc9Op6unaXrXQKSNEADXkZYJ6T5trp1%2Be%2FgJHmQIysU%2BW7aYcXaGK4N9Lc7SHVRIqA4IX1vVw2plCG9z&uid=QchKIdBoOeE1wsFONZ4se/WuF3wDZMRC

![image-20231225231957084](.\Picture\image-20231225231957084.png)

![image-20231225232049622](.\Picture\image-20231225232049622.png)

​	STM32CubeIDE 软件安装完成后同样 ST-Link 驱动也安装完成，ST-Link 连接至 PC，可在设备管理器中看到 ST-link Debug 和 STMicroelectronics STLink Virtual COM Port 串口

![image-20231225233210431](.\Picture\image-20231225233210431.png)

注意：如果设备管理器中 ST-Link 驱动无法识别，带有惊叹号，需在高级启动中禁用驱动程序强制签名

##	2. **STM32CubeIDE** **软件使用**

1.   点击快捷图标,选择工程存储位置.
     ![image-20231225233659197](.\Picture\image-20231225233659197.png)
2.   软件界面
     ![image-20231225233943447](.\Picture\image-20231225233943447.png)
3.   选择开发板上使用的STM32芯片型号
     ![image-20231225234339825](.\Picture\image-20231225234339825.png)

4.   点击finish 填入工程名称

![image-20231225234611315](.\Picture\image-20231225234611315.png)

5.   点击yes进入软件页面
     ![image-20231225234728749](.\Picture\image-20231225234728749.png)

6.   软件页面如下 
     ![image-20231225234822359](.\Picture\image-20231225234822359.png)

7.   搜索框内搜索 LED 对应 GPIO 引脚 PZ5、PZ6、PZ7，左键点击设置为 GPIO_Output
     ![image-20231225235345762](.\Picture\image-20231225235345762.png)

8.   在 Code Generator 处选择为每个外设生成单独的 C 和 H 文件，这样设置方便阅读代码
     ![image-20231225235517741](.\Picture\image-20231225235517741.png)

9.   完成以上设置后，Ctrl+S 保存，会提示是否需要生成代码，选择 Yes 即可自动生成代码。

     系统会自动生成 System Clock 代码

     ![image-20231225235549463](.\Picture\image-20231225235549463.png)

10.    可以在左侧工程文件夹看到生成的工程，CA7 文件夹是给 A7 核使用的。Drivers 文件夹是 ST 提供的 HAL_Drivers，用户无需修改。LED_CM4 子工程是我们生成的 M4 内核的工程代码。
      如果找不到工程代码，那就是缺少软件包
      ![image-20231226002744120](.\Picture\image-20231226002744120.png)

![image-20231226002839047](.\Picture\image-20231226002839047.png)

如果遇到需要login那就验证登录，下载成功如下

![image-20231226002526238](.\Picture\image-20231226002526238.png)

11.   可以成功下载到所需要的工程

![image-20231226002933477](.\Picture\image-20231226002933477.png)

![image-20231226003025193](.\Picture\image-20231226003025193.png)

```A7 文件夹是给 A7 核使用的。Drivers 文件夹是 ST 提供的 HAL_Drivers，用户无需修改。LED_CM4 子工程是我们生成的 M4 内核的工程代码。```

12.    可以在左侧工程文件夹看到生成的工程，CA7 文件夹是给 A7 核使用的。Drivers 文件夹是 ST 提供的 HAL_Drivers，用户无需修改。DTN_STM32_MP157_CM4 子工程是我们生成的 M4 内核的工程代码。

![image-20231226203845416](.\Picture\image-20231226203845416.png)

##	3. 编译下载工程文件

###	3.1. 开发板连接

拨动开发板启动拨码至 001，开发板上电，使开发板处于 Engineering mode，就可以进

行开发或调试 CM4 固件了，连接好 ST-LINK 和开发板

![image-20231226215630015](.\Picture\image-20231226215630015.png)

![image-20231226215652759](.\Picture\image-20231226215652759.png)

###	3.2. 工程编译

![image-20231226220029483](.\Picture\image-20231226220029483.png)

###	3.3.	DEBUG调试

1.   选择DEBUG按键

![image-20231226220230987](.\Picture\image-20231226220230987.png)

 	2.	选择 STM32 Cortex-M C/C++ Application
 	 ![image-20231226220801842](.\Picture\image-20231226220801842.png)

3.   调试器 下选择 thru JTAG/SWD link (Engineering mode) ，完成点击 Debug
     ![image-20231226220912466](.\Picture\image-20231226220912466.png)

4.   选择 Switch
     ![image-20231226222145387](.\Picture\image-20231226222145387.png)

5.点击运行

![image-20231226222521523](.\Picture\image-20231226222521523.png)