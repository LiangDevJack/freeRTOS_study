# 四、FreeRTOS 中断处理和命令行界面

**回顾（Recap）：**

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

## 1、FreeRTOS中断处理

- 中断：由外部事件（如定时器或外围设备）触发时执行的功能
- 中断将立即执行，如果处理不当，将会给任何正在运行的任务造成错误
- 微处理器处理:大多数微处理器会在执行中断服务例程(ISR)之前将处理器寄存器的值保存到堆栈上
    
    对于 RTOS，这不足以防止错误的行为
    
- 回顾：每个任务都有一个任务上下文块（TCB)）包含以下值：
（a）、微处理器寄存器
（b）、任务状态
（c）、任务变量
（d）、堆栈变量
- 如果要中断或挂起一个任务，TCB 需要被保存
- 任务恢复时，TCB 需要恢复
- TCB允许内核暂停并恢复任务

![Untitled](%E5%9B%9B%E3%80%81FreeRTOS%20%E4%B8%AD%E6%96%AD%E5%A4%84%E7%90%86%E5%92%8C%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%95%8C%E9%9D%A2%20d5b908cc28214905894ab56a5572ef39/Untitled.png)

**中断服务程序必须：**

- 处理中断
    
    检查正确的中断条件（即掩码标志mask flags）
    
    清除中断标志
    
- 执行任何任务同步（即触发信号）
- 保存 TCB
检查优先级较高的中断处理任务是否可以执行(上下文切换)
- 恢复TCB
- 内核恢复执行

![Untitled](%E5%9B%9B%E3%80%81FreeRTOS%20%E4%B8%AD%E6%96%AD%E5%A4%84%E7%90%86%E5%92%8C%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%95%8C%E9%9D%A2%20d5b908cc28214905894ab56a5572ef39/Untitled%201.png)

**FreeRTOS信号量回顾：**

- 通常用于同步任务和中断：
    
    任务将阻塞一个信号量，直到中断发出信号
    
- 二进制信号量：
    
    阻塞等待和解除阻塞时发出信号
    

```c
void peripheral_ISR(void)
{
	portBASE_TYPE xHigherPriorityTaskWoken;
	/* Is it time for another Task() to run? */
	xHigherPriorityTaskWoken = pdFALSE;
	xSemaphoreGiveFromISR(xSemaphore,&xHigherPriorityTaskWoken );
	/* Perform context switching, if required. */
	portEND_SWITCHING_ISR( xHigherPriorityTaskWoken );
}
/* Task blocking on semaphore. */
void vTaskCode( void * pvParameters )
{
		for( ;; ) 
		{
			if (xSemaphoreTake(xSemaphore, 10) == pdPASS) 
			{
				….
			}
		}
```

| 函数名称 | 描述 |
| --- | --- |
| xSemaphoreCreateBinary | 创建二进制文件信号 |
| xSemaphoreGiveFromISR | 从中断服务程序中提取一个信号量 |
| xSemaphoreTake | 阻塞任务中的信号量 |
| portEND_SWITCHING_ISR | 恢复任务上下文块或切换到另一个高优先级任务 |
| portYIELD_FROM_ISR | 执行上下文切换 |
| portENABLE_INTERRUPTS | 使能中断 |
| portDISABLE_INTERRUPTS | 禁用中断 |

## FreeRTOS库生态系统

- 独立FreeRTOS开发：FreeRTOS是一个实时内核，提供了有用的RTOS函数，如任务、信号量、队列、软件定时器等。
- 为FreeRTOS提供高级可选功能
    
    （a）、POSIX IO 驱动结构
    
    （b）、命令行界面
    
    （c）、网络通信栈：嵌入式网络服务器
    
    （d）、网络安全
    
    （e）、跟踪调试
    

## FreeRTOS+命令行界面（CLI）

- FreeRTOS+ 命令行界面 (CLI)
    
    （a）、简单，小型，可扩展的FreeRTOS应用程序库，以处理命令行输入
    
    （b）、通过终端窗口，用户可以直接执行命令
    
- 类似于 Linux\UNIX 上的 Bash 终端，但简单得多

## FreeRTOS+CLI执行

- 创建一个函数来执行你的应用程序
- 使用 CLI 创建命令函数
- 将命令函数映射到应用程序
- 在CLI下注册命令功能
- 执行 CLI 任务接收输入字符串：当接收到‘\n’时，只处理输入

**注：**

Parse Serial Input                          解析串行输入

Extract Command                         提取命令

![Untitled](%E5%9B%9B%E3%80%81FreeRTOS%20%E4%B8%AD%E6%96%AD%E5%A4%84%E7%90%86%E5%92%8C%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%95%8C%E9%9D%A2%20d5b908cc28214905894ab56a5572ef39/Untitled%202.png)

**FreeRTOS+ CLI Echo 命令：**

```c
//Initialise CLI command input struct
static const xCommandLineInput xEcho = {
( const portint8_t * const ) "echo",
( const portint8_t * const ) "echo: Echo the input.\r\n",
prvEchoCommand,0};
static portBASE_TYPE prvEchoCommand(int8_t *pcWriteBuffer, size_t
xWriteBufferLen, const int8_t *pcCommandString ) {
xWriteBufferLen = sprintf(pcWriteBuffer, ”%s”,
FreeRTOS_CLIGetParameter(pcCommandString));
return pdTRUE;
}
int main() {
……
FreeRTOS_CLIRegisterCommand(&xEcho);
…
return 0;
}

Void USB_CLI_task() {
//Receive string from USB – wait until
‘\n’ is received.
….
xReturned =
FreeRTOS_CLIProcessCommand( cInputString,pcOutputString,
configCOMMAND_INT_MAX_OUTPUT_SIZE );
….
//Display CLI output.
}
```

| 传输模式 | 描述 |
| --- | --- |
| FreeRTOS_CLIRegisterCommand | 注册一个命令 |
| FreeRTOS_CLIProcessCommand(int8_t*pcCommandInput,int8_t*pcWriteBuffer,size_txWriteBufferLen ); | 处理串行输入字符 |
| FreeRTOS_CLIGetParameter | 从串行输入字符串提取参数 |
| xFunctionName(int8_t*pcWriteBuffer,size_txWriteBufferLen,const int8_t*pcCommandString ); | 命令功能——执行命令 |

具体内容：[https://freertos.org/FreeRTOS_Plus/FreeRTOS_Plus_CLI/FreeRTOS_Plus_Command_Line_Interface.html](https://freertos.org/FreeRTOS-Plus/FreeRTOS_Plus_CLI/FreeRTOS_Plus_Command_Line_Interface.html)