# 一、FreeRTOS入门

---

## 一、知识学习

板子相关资源

[https://os.mbed.com/platforms/ST-Nucleo-F401RE/#board-pinout](https://os.mbed.com/platforms/ST-Nucleo-F401RE/#board-pinout)

### 1、代码移植

1）、工程文件里创建放FreeRTOS相关文件的文件夹

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled.png)

2）、Keil软件添加工程文件组并添加文件

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%201.png)

3）、添加头文件路径

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%202.png)

### 2、ST-Nucleo-F401RE引脚图

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%203.png)

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%204.png)

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%205.png)

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%206.png)

**LED1→PA5**                                                                                                                                                   

**按键→PC13**                                                                                                                                              

## 三、遇到的问题

### 1、缺少FreeRTOSConfig.h文件

**解决办法**

从官方例程中寻找配置文件，添加到include里

### 2、**重复宏定义**

TEST1\TEST1.axf: Error: L6200E: Symbol SVC_Handler multiply defined (by port.o and stm32f4xx_it.o).

**重复宏定义，注释掉即可。**                                                                                                                       

```c
//#define vPortSVCHandler    SVC_Handler
//#define xPortPendSVHandler PendSV_Handler
```

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%207.png)

### 3、选用Debug方式（未解决）

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%208.png)

### 3、时钟的选择（内部/外部）

### 4、编译下载环境

### **编译**

在平台文件夹内，应该有一个名为的文件`main.c`和一个名为`Makefile`. 执行命令： `make`

所有的 c 文件现在应该开始构建，当它完成时，将有一个 main.bin 文件、main.elf 文件、main.hex 文件和 main.o 文件，以及一个 obj 目录。

### **闪烁代码**

现在我们准备刷机，导航回到开始，闪烁的makefile并执行

```
make flash

```

接受任何弹出窗口，闪光灯现在将循环通过比较、擦除、编程、验证步骤，之后 Nucleo 上的 LED 应该开始闪烁。

### **提示和故障排除**

- `make clean`
    
    清理已编译的二进制文件，这在重新编译代码时并不总是必要的，因为并非所有内容都需要重新编译，但是在进行大量更改时它很有用。
    
- `make -j`
    
    通过使用所有分配的 cpu 内核使编译速度更快，请注意，当使用 VM 充分利用这一点时，您需要在 VM 设置中分配更多 CPU 内核
    
- if `make` complains about a `.depend` target, you open the makefile and change `include .depend` to `-include .depend`

### **更新刷机工具**

本课程中使用的刷机工具依赖于 JLink 库，JLink 用于与板上的调试芯片通信，然后将代码刷写（以及做其他事情）。但是板 dy 默认带有不同的调试芯片，ST -LINK，因此我们需要将 ST-LINK 固件替换为 JLink 固件。如果您是内部学生，这可能已经为您完成，您可以尝试跳到下一部分，外部学生应该继续阅读。

在 Windows 机器（非 VM）上执行以下步骤。

- 下载并安装适用于 Windows [ST-LINK的 ST-LINK USB 驱动程序](https://www.st.com/en/development-tools/stsw-link009.html)
- 下载并安装最新的 Windows [JLink 固件](https://www.segger.com/downloads/jlink/)
- [从Segger](https://www.segger.com/downloads/jlink/#STLink_Reflash)下载 ST-Link 刷新实用程序

运行刷新器 -> 接受 -> [1] 升级到 J-Link -> [0] 退出

### **验证 JLink**

在您的虚拟机中，确保已插入 nucleo 并作为设备连接，通过使用`sudo dmesg`并查看新连接设备的日志底部进行验证，并在插入 USB 时查找 /dev/ttyACM0 和设备已连接到 VM。（请注意，nucleo 不能同时连接到 VM 和主机操作系统，因此如果它在您的主机操作系统中可见，则它不会连接到您的 VM）

注：运行刷新器时，确保与主机相连接，否则则更新不了。                                                           

在Windows上装的是ST的驱动，板子上原来装的出厂的固件儿是ST的固件儿，它在那个Windows上能识别，然后你刷完之后，它这个固件儿就不是ST的了，是这Jlink了，然后这Jlink在Linux上有，所以说在Linux下编译。

![Untitled](%E4%B8%80%E3%80%81FreeRTOS%E5%85%A5%E9%97%A8%20228fb3185f2a42afaf9caf36a51c9771/Untitled%209.png)

### 5、补充

**Linux常用命令**

1）、cd : 改变目录，进入下个目录。Table：自动补全目录。

2）、cd . . 回退到上一个目录，直接cd进入默认目录。

3）、pwd : 显示当前所在的目录路径。

4）、ls(ll): 都是列出当前目录中的所有文件，只不过l

5）、touch : 新建一个文件 如 touch index.js 就会在当

6）、rm: 删除一个文件, rm index.js 就会把index.js文

7）、mkdir: 新建一个目录,就是新建一个文件夹

**makefile文件**

makefile: 一个工程中的源文件不计其数，其按类型、功能、模块分别放在若干个目录中，makefile定义了一系列的规则来指定哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作，因为 makefile就像一个Shell脚本一样，也可以执行操作系统的命令