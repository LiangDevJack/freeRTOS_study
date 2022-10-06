# 三、FreeRTOS介绍&基础

**1、使用rtos比使用单个线程C库有优势**

**2、RTOS由一个实时调度、上下文切换和资源管理的内核组成**

**3、RTOS提供，执行的线程，同步（Synchronisation），参数共享**

（1）、执行的线程（任务）：

- 小型、可调度和顺序可编程单元
- 并发操作和多任务处理

（2）、同步（信号量）：

- 允许任务被阻止或暂停，直到发出信号
- 提供互斥

（3）、参数共享（队列）：

- 用于在任务之间共享变量
- 允许通过多任务的方式，同步读取/写入项目（即防止多次写入错误）

（4）、FreeRTOS 是一个实时内核

实时调度

任务间通信

时序原语

同步原语

注：

原语(primitives)：

       在操作系统中，某些被进程调用的操作，如队列操作、对信号量的操作、检查启动外设操作等，一旦开始执行，就不能被中断，否则就会出现操作错误，造成系统混乱。所以，这些操作都要用原语来实现 原语是操作系统核心（不是由进程，而是由一组程序模块组成）的一个组成部分，并且常驻内存，通常在管态下执行。原语一旦开始执行，就要连续执行完，不允许中断 。

       原语是在操作系统中调用核心层子程序的指令。与一般广义指令的区别在于它是不可中断的，而且总是作为一个基本单位出现。它与一般过程的区别在于：它们是“[原子操作](https://baike.baidu.com/item/%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C/1880992)（primitive or atomic action）”。所谓原子操作，是指一个操作中的所有动作要么全做，要么全不做。换言之，它是一个不可分割的基本单位，因此，在执行过程中不允许被中断。原子操作在管态下执行，常驻内存。原语的作用是为了实现进程的通信和控制，系统对进程的控制如不使用原语，就会造成其状态的不确定性，从而达不到进程控制的目的。

       同步原语是平台(例如操作系统)为其用户提供的简单软件机制，目的是支持线程或进程同步.它们通常是使用较低级的机制(例如原子操作，内存屏障，自旋锁，上下文切换等)构建的.

（5）、附加功能

- 命令行界面
- 网络堆栈
- 文件系统

**FreeRTOS 简介**

- 特征
- 平台

**FreeRTOS 内核**

- FreeRTOS 调度程序
- FreeRTOS 计时

**代码语法格式**

**资源**

## FreeRTOS特征

- 抢先式（Pre-emptive）或合作式调度选项
- 易于使用的消息传递
- 带时间切片的轮询（Round robin）
- 可只读存储器
- 具有优先级继承的互斥对象
- 6K 至 10K ROM 占用空间
- 递归互斥体（Recursive Mutexes）
- 可配置/可扩展
- 二进制和计数信号量
- 编译程序的诊断程序（Compiler Diagnostics）
- 高性能的软件定时器
- 某些端口不完全禁用中断
- 易于使用的API

### FreeRTOS背景

- FreeRTOS 大约在 2003 年开始
- 由 Real Time Engineers Ltd（英国）创建
- 免费下载和使用
- License：修改后的GPL，可商用应用程序
- 专为Windows 开发而设计
- SafeRTOS 变体经认证可用于安全关键应用程序。

### FreeRTOS平台

- 专为资源受限的平台而设计
- 移植到超过 31 种处理器架构：

ARM7，Cortex M series，Atmega32u4，MSP430，8051

### FreeRTOS内核（Kernel）

**特点：**

- 抢占式或并发调度器
- 可配置堆管理
- 记忆保护

**提供以下原语：**

- 任务（执行的线程）：协程（Co-routine）
- 信号量（二进制和计数）
- 队列（参数共享）
- 中断处理（线程安全函数）

### RTOS调度器（Scheduler）

**优先的抢先调度：**

- 优先级 - 每个任务都有一个优先级，即预先确定（可以更改）。
- 抢先 - 任务可以在等待事件发生。
- 事件同步由信号量控制。
- 事件可以由其他任务发出。
- 可选的合作调度模式：上下文切换仅在执行任务时发生被锁住。

**优先的抢先调度示例：**

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled.png)

注：

pre-empts      预先制止；抢在……之前行动；抢先得到

## FreeRTOS时序

- FreeRTOS 实时内核使用滴答计数变量
- 定时器中断以严格的方式增加滴答计数时间精度
- 允许实时内核测量时间所选计时器中断频率的分辨率
- 每次滴答计数实时递增内核必须检查现在是否该解除阻塞或唤醒任务
- 任务可能在滴答声中被唤醒或解除阻塞，ISR 的优先级高于中断的任务：

              滴答 ISR 应该返回到新的唤醒/解锁任务 - 有效地打断一个任务

**注：**

Interrupt Service Routines（中断服务程序）

**RTOS滴答举例：**

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%201.png)

**注：**

Pseudo code   伪代码

ready→readied  已准备的

increment   增量、v. （计算机）使（数字值）各自增加

(1) 、RTOS空闲任务正在执行

(2)、RTOS 滴答发生，控制转移到滴答 ISR

(3)、RTOS 滴答 ISR 使 vControlTask 准备好运行，并且作为vControlTask 的优先级高于 RTOS 空闲任务，切换vControlTask 的上下文

(4)、由于执行上下文的现在是 vControlTask，因此退出ISR 将控制权返回给 vControlTask，它开始执行 

(5)、像这样发生的上下文切换是抢先的，因为被中断的任务是被抢占而不是自动挂起

```c

/***********定时器中断**************/
/* Interrupt service routine for the RTOS tick. */
void SIG_OUTPUT_COMPARE1A( void ) {
/* Call the tick function. */
vPortYieldFromTick();
/* Return from the interrupt.*/
asm volatile ( "reti" );
}

/***********生成Tick****************/
void vPortYieldFromTick( void ) {
portSAVE_CONTEXT();
/* Increment the tick count */
vTaskIncrementTick();
/* See if a context switch is required. */
vTaskSwitchContext();
/* Restore the context. */
portRESTORE_CONTEXT();
asm volatile ( "ret" );
}

```

## 代码语法风格（Code Syntax Style）

**变量名称（计数器除外）：前缀名称代表的类型**

‘c’ 代表 char, ‘s’ 代表 short, ‘l’ 代表 long, ‘x’代表portBASE_TYPE,

‘u’ 代表 unsigned

‘p’ 代表 pointer

i.e. unsigned char accelerometer_x

应该被命名: ucAccelerometer_x

**函数名称：带有返回类型和文件名的前缀名称**

'v' 代表 void，'x' 代表 portBASE_TYPE

即xtaskCreate - 返回portbase_type，in task.c

**宏名称:应该是大写。必须以小写的返回类型和文件名前缀**

**文件名字：**

Portmacro.h – port

Task.h      —task

Projdef.h    — pd

Projdef.h — err

FreeRTOSConfig — config
**例子**

pdTRUE    1

pdFALSE   0

pdPASS     1

pdFAIL       0

数据类型：有                               portTickType
唯一数据类型                               portBASE_TYPE
在 portmacro.h 中定义                 portMAX_delay

格式化                                           使用4tab空格

免费**实时操作系统工具\资源**

[www.freeRTOS.org](http://www.freertos.org/)

**总结（Summary）**

- FreeRTOS 是一个实时内核：提供任务、信号量、队列和内存堆控制
- FreeRTOS 调度程序基于优先级抢占的
- FreeRTOS定时依赖于一个单一的定时器中断来生成定时滴答

FreeRTOS 是一个实时内核：

- 实时调度
- 任务间通信
- 同步原语
- 时序
- 内存分配

## FreeRTOS任务

- 实时应用程序可以构建为一组的任务
- 任何时候只能执行一项任务时间
- 在应用程序执行时，调度程序可以启动和停止每个任务(切换每个任务)
- 每个任务都有自己的内存栈（TCB – Task控制块）
- 当任务交换出时，执行上下文被保存到该任务的堆栈中，因此当稍后交换回相同的任务时，它也可以完全恢复
- 任务被分配一个优先级，调度程序使用它来决定执行或挂起哪个任务

## FreeRTOS任务控制块（Task Control Block）

- TCB - 任务控制块（或任务的'堆'）：任务使用的内存块
- 用于保存任务的：局部变量、处理器寄存器的当前值、当前任务状态
- 用于上下文切换：挂起或恢复一个任务、确保上下文切换后不会发生故障、允许在不知道的情况下挂起或恢复任务

## **FreeRTOS 任务状态**

一个任务可以存在于以下其中之一状态：

- **运行（Running）：**当一个任务正在实际执行时，我们称它处于Running状态，并且正在使用CPU
- **准备就绪（Ready）：**就绪任务是那些能够执行但目前没有执行的任务，因为另一个具有同等或更高优先级的任务已经处于运行状态
- **阻塞（Blocked）：**

（i）、任务正在等待事件

**临时事件:**   一个任务调用vTaskDelay()，它将阻塞(被放置到阻塞状态)，直到延迟期过期。

**外部事件：**任务也可以阻塞等待队列和信号量事件。

（ii）、 处于阻塞状态的任务总是有一个超时，超时后该任务将被解除阻塞。

（iii）、阻塞的任务无法进行调度。

- **挂起（Suspended）：**

（a）、使用vTaskSuspend()和xTaskResume()，任务只能进入或退出挂起状态。

（b）、不能指定“超时”时间

（c）、被挂起的任务无法被调度。   

**任务状态图表**

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%202.png)

## 任务优先事项

每个任务被分配一个从0到configMAX PRIORITIES 1的优先级(FreeRTOSConfig.h)。

低优先级数字表示低优先级任务。

空闲任务的优先级为零(tskIDLE_PRIORITY)。

处于运行状态的任务始终是能够运行的最高优先级任务。

对于同等优先级的就绪任务，采用时间切片轮询调度方案进行调度。

**任务应具有以下结构：**

```c
void vATaskFunction( void *pvParameters )
{
	for( ;; )
		{
				-- Task application code here. --
		}

```

**任务使用 xTaskCreate() 创建并使用 vTaskDelete() 删除：**

```c
portBASE_TYPE xTaskCreate (pdTASK_CODE pvTaskCode,const portCHAR * const pcName,
unsigned portSHORT usStackDepth,void pvParameters,unsigned portBASE_TYPE uxPriority,
xTaskHandle pvCreatedTask);
//**创建不带参数的任务示例：**
xTaskCreate(vTaskCode, "NAME", STACK_SIZE, NULL,tskIDLE_PRIORITY，NULL）；
```

## FreeRTOS空闲任务

- 空闲任务在RTOS调度器启动时自动创建
- 确保至少有一项任务可以运行
- 最低优先级，确保当其他高优先级的应用程序任务处于就绪状态时，它不会占用任何CPU时间。
- 用于释放分配给已删除任务的内存
- 空闲任务必须有其他活动的函数，这些函数在所有条件下都可能消耗CPU时间（即闪烁LED灯）。
- 可以在Freertosconfig.h文件里启用/禁用空闲任务。

## FreeRTOS任务函数调用

| 函数名称 | 描述 |
| --- | --- |
| xTaskCreate | 创造一个任务 |
| vTaskDelete | 删除一个任务 |
| vTaskDelay | 延迟任务一段时间 |
| vTaskDelayUntil | vTaskDelay函数的一种变体 |
| vTaskPrioritySet | 设置任务优先级 |
| uxTaskPriorityGet | 获取一个任务的优先级 |

## FreeRTOS 协同程序

- 协同例程类似于任务，但并不相同
- 堆栈使用：

应用程序中的所有协同例程共享一个堆栈。与使用任务相比，这可以减少所需的RAM数量。

- 调度和优先级：

（1）、相对于其他协同程序，使用优先级协同调度

（2）、调度程序必须重复调用

- 在协程之间共享堆栈，导致 RAM 使用率低得多。
- 减少可重入性（Reentrancy）问题
- 便携式跨架构
- 相对于其他协程完全优先，但总是可以被任务抢占。
- 对API调用位置的限制
- 只在协程之间进行协作操作

## FreeRTOS 协同程序状态

- **运行：**使用 CPU 的协程。
- **准备就绪：**

（1）、协同例程是那些能够执行(而不是被阻塞)但当前没有执行的例程。

（2）、协程可能处于就绪状态

**因为：**
（a）、另一个同等或更高的协程优先级已经处于运行状态。
（b）、任务处于运行状态。

- **阻塞：**
如果协同例程当前正在等待一个暂时事件或外部事件，则它处于阻塞状态。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%203.png)

```c
void vACoRoutineFunc(xCoRoutineHandle xHandle,unsigned portBASE_TYPE uxIndex )
{
	crSTART( xHandle );
	for( ;; )
	{
		-- Co-routine application code here. --
	}
	crEND();
}
```

## FreeRTOS 调度协同程序

- 通过重复调用vCoRoutineSchedule()来调度协同程序
- 空闲任务用于调用vCoRoutineSchedule()

## FreeRTOS信号量

- 信号量用于同步和互斥
- 信号量允许指定块时间：阻塞时间表示任务应该进入阻塞状态的“滴答”的最大数量
- 如果在同一个信号量上有多个任务阻塞，那么优先级最高的任务将第一个被解除阻塞
- 通常用于同步任务和中断：任务将阻塞一个信号量，直到中断发出信号
- **三种类型：**
    
    二进制信号量：阻塞等待并在有信号时解除阻塞
    
    计数信号量：发出信号时返回一个计数器值
    
    互斥体：用于提供互斥
    
- 二进制信号量用于互斥和同步目的
- **与互斥锁类似，但主要区别在于：**

（1）、互斥锁包含优先级继承机制，而二进制信号量没有：

优先级继承允许互斥对象拥有持有该互斥对象的任务的优先级

（2）、二进制信号量主要用于同步（任务之间或之间任务和中断）

- 互斥锁用于简单的互相排斥

注：

**Semaphore           信号量**

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%204.png)

...解除任务阻塞(信号量现在是可用的)。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%205.png)

...现在成功地“获取”了信号量，所以再次是无法使用的。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%206.png)

该任务现在可以运行，直到需要获得信号量增益（gain），这将使它进入阻塞（block）状态。

## FreeRTOS互斥体（Mutex）

注：

**互斥体（Mutex）：**

       互斥体实现了“互相排斥”（mutual exclusion）同步的简单形式（所以名为互斥体(mutex)）。互斥体禁止多个线程同时进入受保护的代码“临界区”（critical section）。

       在任意时刻，只有一个线程被允许进入代码保护区。任何线程在进入临界区之前，必须获取（acquire）与此区域相关联的互斥体的所有权。如果已有另一线程拥有了临界区的互斥体，其他线程就不能再进入其中。这些线程必须等待，直到当前的属主线程释放（release）该互斥体。互斥体用于保护共享的易变代码，也就是，全局或静态数据。这样的数据必须通过互斥体进行保护，以防止它们在多个线程同时访问时损坏。

**互斥锁：**

        在编程中，引入了对象互斥锁的概念，来保证共享数据操作的完整性。每个对象都对应于一个可称为" 互斥锁" 的标记，这个标记用来保证在任一时刻，只能有一个线程访问该对象。

**Mutex                                                   互斥体**

**Guarded Resource                               被守卫的资源/保护资源**

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%207.png)

1.、两个任务想要需要使用相同的资源，需要在资源可以被访问（access）之前保存互斥。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%208.png)

2、任务 A 尝试获取互斥体，并现在持有**它**且访问资源。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%209.png)

3、任务 B 尝试获取互斥体，但被阻塞，无法访问资源。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2010.png)

4. 任务 B 进入（阻塞）Block状态，等待获取互斥体。任务A将继续运行，直到完成并释放互斥体。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2011.png)

5、任务 A 释放互斥体，任务 B 可以解除阻塞，获取互斥体并访问资源。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2012.png)

6、任务 B 完成并释放互斥体，任何任务都可以自由获取互斥体。

## FreeRTOS计数信号量

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2013.png)

1、任务阻塞，等待获取信号量。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2014.png)

2、中断发生并给出信号量

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2015.png)

3、现在可以获取信号量，任务将解除阻塞。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2016.png)

4、任务接受信号量。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2017.png)

5、当任务处理第一个事件时，另外两个中断发生。

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2018.png)

6、 有了两个可用的信号量，该任务将会接受一个信号量，而不会进入阻塞状态。另一个信号量处于可获取状态。

```c
void peripheral_ISR(void) {
		…
		xSemaphoreGiveFromISR(xBinary_semaphore);
		}
		// Task blocking on semaphore.
		void vTaskCode( void * pvParameters ) {
		for( ;; ) {
		if (xSemaphoreTake(xBinary_semaphore,
		10) == pdPASS) {
		….
		}
}
```

## FreeRTOS信号量（Semaphore）函数调用

| 函数名称 | 描述 |
| --- | --- |
| xSemaphoreCreateBinary | 创建一个二进制信号量 |
| xSemaphoreCreateCounting | 创建计数信号量 |

| xSemaphoreGiveFromISR | 从中断服务程序中提取（single）一个信号量 |
| xSemaphoreTake | 阻塞处理任务中的信号量。 |

注：

**信号量：**

     （英语：**semaphore**）又称为**信号标**，是一个同步对象，用于保持在0至指定最大值之间的一个计数值。当线程完成一次对该semaphore对象的等待（wait）时，该计数值减一；当线程完成一次对semaphore对象的释放（release）时，计数值加一。当计数值为0，则线程等待该semaphore对象不再能成功直至该semaphore对象变成signaled状态。semaphore对象的计数值大于0，为signaled状态；计数值等于0，为nonsignaled状态。

       在系统中，给予每一个进程一个信号量，代表每个进程目前的状态，未得到控制权的进程会在特定地方被强迫停下来，等待可以继续进行的信号到来。如果信号量是一个任意的整数，通常被称为计数信号量（Counting semaphore），或一般信号量（general semaphore）；如果信号量只有二进制的0或1，称为二进制信号量（binary semaphore）。

## FreeRTOS队列（Queue）

- 用于任务间通信
- 用于在任务之间、中断和任务之间发送消息：即 FIFO（先进先出）缓冲区
- 被认为是线程安全的 FIFO 缓冲区
- 队列可以包含固定大小的“项（items）”
- 队列项可以添加到队列的前面或后面
- 项目通过复制，而不是通过引用放置到队列中：需要将放入队列中的每个项目的大小保持在最小
- 多个任务不能同时访问数据
- 一个队列得确保互斥
- 队列API允许指定阻塞时间
- 如果在同一个队列上有多个任务阻塞，那么优先级最高的任务将是最先被解除阻塞的任务

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2019.png)

1、创建队列

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2020.png)

2、任务A将局部变量写到队列后面

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2021.png)

3、任务 A 将局部变量的新值写入队列

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2022.png)

4、任务B从队列中读取（接收）

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2023.png)

5.、任务 B 从队列中移除项目

```c
void vSender_Task( void * pvParameters )
{
	unsigned short usValueToSend;
	for( ;; ) 
	{
				…
		xStatus = xQueueSendToback(xQueue, &usValueToSend, 0);}
	}
}

void vReceiver_Task( void * pvParameters ) 
{
	unsigned short usValueRead;
	for( ;; ) 
	{
		xStatus = xQueueReceive(xQueue, &usValueRead, 100) ;
		if (xStatus == pdPASS) {
…
}
```

## FreeRTOS队列函数调用

| 函数名称 | 描述 |
| --- | --- |
| xQueueCreate | 创建队列 |
| xQueueSendToBack | 发送一个项目到队列的后面 |
| XQueueSendToFront | 发送一个项目到队列的前面 |
| xQueueReceive | 从队列中接收和移除项目(阻塞) |
| xQueuePeek | 收到但不删除队列中的项目 |
| uxQueueMessagesWaiting | 返回队列中项目（items）的数量 |

## FreeRTOS软件定时器

- 软件定时器允许在设定的时间后执行一个函数
- 软件定时器执行的函数被称为使用软件定时器的回调函数
- 从软件计时器启动到它的回调函数执行之间的时间称为周期
- 当软件定时器的时间到期时，执行软件定时器的回调函数

| 函数名称 | 描述 |
| --- | --- |

| xTimerIsActiveTimer | 检查是否软件计时器处于活动状态 |
| pvTimerGetTimerID | 返回软件定时器的ID |
| xTimerStart | 启动定时器 |
| xTimerStop | 停止定时器 |
| xTimerReset | 重置定时器 |

## FreeRTOS内存管理

- 每次创建任务、队列、互斥锁、软件计时器或信号量时都会用到内存(RAM)
- FreeRTOS 有四个样本内存分配实现：
    
    堆 1：不允许释放内存
    
    堆 2：最佳拟合算法（Algorithm）并允许释放内存
    
    堆 3：实现 Malloc 和 Free函数功能
    
    堆 4/5：第一次拟合（First Fit）算法，并允许释放内存
    

### 1、FreeRTOS 堆（Heap）1

- 一旦被分配，就不允许释放内存
- 如果应用程序从来没有删除任务、队列、信号量、互斥锁等，可以使用这个选项
- 是确定性的(执行总是花费相同的时间)
- 当RAM被请求时，通过将单个数组细分为更小的块来实现

TCB – Task Control Block

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2024.png)

### 2、FreeRTOS 堆（Heap）2

- 使用最适合的内存分配算法。
- 允许释放先前分配的块
- 不将相邻的空闲块组合成一个单个大块
- 即使在应用程序重复删除任务、队列、信号量、互斥锁等时也可以使用
- 不确定性
- 适用于大多数需要动态创建任务的应用程序
- 如果分配和释放的内存(任务堆栈和队列大小)是随机大小，则不应该使用

![Untitled](%E4%B8%89%E3%80%81FreeRTOS%E4%BB%8B%E7%BB%8D&%E5%9F%BA%E7%A1%80%2099e81c5a14c4491db06e052bd083f558/Untitled%2025.png)

### 3、FreeRTOS 堆（Heap）3

- 实现标准的包装器malloc() 和 free() 函数
- 包装器简单地使 malloc()和 free() 函数线程安全
- 要求链接器建立一个堆，并且提供 malloc() 的编译器库和 free() 实现
- 不确定性
- 如果分配和释放的内存(任务堆栈和队列大小)是随机大小，可以会被使用

### 4、FreeRTOS 堆（Heap）4/5

- 使用第一次拟合内存分配算法
- 将相邻的空闲内存块合并成一个大的块
- 包括一个合并算法
- 即使应用程序反复删除任务、队列、信号量、互斥体等，也可以使用
- 即使分配和释放的内存大小是随机的，也不太可能导致堆空间被严重碎片成多个小块
- 不确定性

**注：**

包装器（Wrapper）：一种构造，例如[类](https://en.wiktionary.org/wiki/class)或[模块](https://en.wiktionary.org/wiki/module)，用于[调解](https://en.wiktionary.org/wiki/mediate)对另一个的访问。

## FreeRTOS堆函数调用

| 函数名称 | 描述 |
| --- | --- |
| configTOTAL_HEAP_SIZE | 设置可用内存的总量(FreeRTOSConfig.h) |
| xPortGetFreeHeapSize | 返回未分配内存的数量 |
| pvPortMalloc | malloc() 的包装器（堆 3 和 4） |
| pvPortFree | free()的包装器（堆 3 和 4） |

## FreeRTOS核心控制

| 宏名称 | 描述 |
| --- | --- |
| taskYIELD | 强制上下文切换 |
| taskENTER_CRTICAL | 标记关键代码区域的开始，在关键区域不能发生上下文切换 |
| taskEXIT_CRTICAL | 标记关键代码（Critical Code）的结尾地区 |
| taskENABLE_INTERRUPTS | 启用中断 |
| taskDISABLE_INTERRUPTS  | 禁用中断 |
| portSAVE_CONTEXT | 保存任务的上下文 |
| portRESTORE_CONTEXT | 恢复上下文 |
| vTaskStartScheduler | 开始调度器，必须从main函数调用 |
| vTaskEndScheduler | 停止调度程序 |
| vTaskSuspendAll | 暂停所有内核活动，但允许所有中断发生 |
| vTaskResumeAll | 恢复所有内核活动 |

**总结：**

- 任务：作为并发的顺序使用编程单元
- 协程：类似于任务，但共享堆栈在协程程序上
- 信号量：提供同步和互斥锁（Mutual Exclusion）
    
    类型：二进制，计数和互斥（Mutual）
    
- 队列：用于在任务之间提供消息传递
- 软件定时器：使函数在一段时间后执行
- 内存管理：
    
    （1）、堆 1：不允许释放内存
    
    （2）、堆 2：最佳拟合算法并允许释放内存
    
    （3）、堆 3：实现 Malloc 和 Free功能
    
    （4）、First Fit 算法并允许释放内存
    
- 内核管理：用于暂停和重新启动内核活动的各种宏和函数