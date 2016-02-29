#lec 3 SPOC Discussion

##**提前准备**
（请在周一上课前完成）

 - 完成lec3的视频学习和提交对应的在线练习
 - git pull ucore_os_lab, v9_cpu, os_course_spoc_exercises  　in github repos。这样可以在本机上完成课堂练习。
 - 仔细观察自己使用的计算机的启动过程和linux/ucore操作系统运行后的情况。搜索“80386　开机　启动”
 - 了解控制流，异常控制流，函数调用,中断，异常(故障)，系统调用（陷阱）,切换，用户态（用户模式），内核态（内核模式）等基本概念。思考一下这些基本概念在linux, ucore, v9-cpu中的os*.c中是如何具体体现的。
 - 思考为什么操作系统需要处理中断，异常，系统调用。这些是必须要有的吗？有哪些好处？有哪些不好的地方？
 - 了解在PC机上有啥中断和异常。搜索“80386　中断　异常”
 - 安装好ucore实验环境，能够编译运行lab8的answer
 - 了解Linux和ucore有哪些系统调用。搜索“linux 系统调用", 搜索lab8中的syscall关键字相关内容。在linux下执行命令: ```man syscalls```
 - 会使用linux中的命令:objdump，nm，file, strace，man, 了解这些命令的用途。
 - 了解如何OS是如何实现中断，异常，或系统调用的。会使用v9-cpu的dis,xc, xem命令（包括启动参数），分析v9-cpu中的os0.c, os2.c，了解与异常，中断，系统调用相关的os设计实现。阅读v9-cpu中的cpu.md文档，了解汇编指令的类型和含义等，了解v9-cpu的细节。
 - 在piazza上就lec3学习中不理解问题进行提问。

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 1. 描述PXE的大致启动流程。

## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。
 1. 了解GRUB的启动流程。
 1. 比较NTLDR和GRUB的功能有差异。
 1. 了解u-boot的功能。

## 3.3 中断、异常和系统调用比较
 1. 什么是中断、异常和系统调用？
 2. 中断、异常和系统调用的处理流程有什么异同？
 2. 举例说明Linux中有哪些中断，哪些异常？
 3. 以ucore lab8的answer为例，uCore的时钟中断处理流程。
 1. Linux的系统调用有哪些？大致的功能分类有哪些？  (w2l1)

 ```
  + 采分点：说明了Linux的大致数量（上百个），说明了Linux系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
 1. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？(w2l1)
 
 ```
  + 采分点：说明了ucore的大致数量（二十几个），说明了ucore系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(w2l1)

1）objdump 反编译，查看目标二进制可执行文件的汇编码
2）nm 列出目标文件的符号清单
3）file 查看目标文件类型，对于可执行文件可以输出运行平台、操作平台以及hash码等
输出：lab1-ex0.exe: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=cc5f079c8185b94784d4d763bd35187f93b22b4e, not stripped
4）strace 跟踪进程执行时的系统调用和所接收的信号，显示程序进程中的调用、参数和返回值

linux通过int 0x80指令来实现系统调用，传参和返回方式是通过寄存器。
HelloWorld中各系统调用的含义：
（1）execve：创建进程
（2）brk：设置程序使用的数据段
（3）access：检测文件的可用性
（4）mmap2：创建文件到内存的映射，单位为page_size
（5）open：打开文件
（6）close：关闭文件
（7）read：输入
（8）write：输出
（9）mprotect：修改文件的访问方式（权限）
（10）exit_group:程序返回
（11）fstate64：获取文件状态
 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 
 ```
 
 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(w2l1)
 
strace -c 统计每一系统调用的所执行的时间,次数和出错的次数等.
-c count time, calls, and errors for each syscall and report summary

/proc/interrupts 文件列出每个 I/O 设备中每个 CPU 的中断数，每个 CPU 核处理的中断数，中断类型，以及用逗号分开的注册为接收中断的驱动程序列表。

brk() sets the end of the data segment to the value specified by end_data_segment, when that value is reasonable, the system does have enough memory and the process does not exceed its max data size.

调用过程：
程序先通过execve创建进程，通过brk设置数据栈大小，通过access、open、fstate、read、close等操作实现连接动态链接库，然后通过mmap和munmap将相应文件以byte为单位映射和取消映射到内存中。通过arch_prctl设置架构特定的线程状态，mprotect控制允许访问的内存区域，最后调用write输出"hello world\n"，然后调用exit_group退出所有线程。

 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 
void
syscall(void) {
    struct trapframe *tf = current->tf;
    uint32_t arg[5];
    int num = tf->tf_regs.reg_eax;
    if (num >= 0 && num < NUM_SYSCALLS) {
        if (syscalls[num] != NULL) {
            // 寄存器之前都已入堆栈，读取堆栈中的参数
            arg[0] = tf->tf_regs.reg_edx;
            arg[1] = tf->tf_regs.reg_ecx;
            arg[2] = tf->tf_regs.reg_ebx;
            arg[3] = tf->tf_regs.reg_edi;
            arg[4] = tf->tf_regs.reg_esi;
            // 结果存到寄存器a对应的堆栈中，系统调用结束时堆栈结果恢复到寄存器，返回结果到寄存器a
            tf->tf_regs.reg_eax = syscalls[num](arg);
            return ;
        }
    }
    print_trapframe(tf);
    panic("undefined syscall %d, pid = %d, name = %s.\n",
            num, current->pid, current->name);
}

 1. 以getpid为例，分析ucore的系统调用中返回结果的传递代码。
 
syscall中sys_getpid返回pid给 结果存到堆栈中对应寄存器a的位置 返回应用程序的时候，堆栈结果恢复到寄存器，结果存到寄存器a中

 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
 
    [SYS_exit]          //退出进程
    [SYS_fork]          //复制创建子进程
    [SYS_wait]          //等待进程结束
    [SYS_exec]          //加载其他可执行程序
    [SYS_yield]         //让出CPU时间
    [SYS_kill]          //删除进程
    [SYS_getpid]        //获得进程号
    [SYS_putc]          //输出一个字节
    [SYS_pgdir]         //返回页目录起始地址
    [SYS_gettime]       //获得时间
    [SYS_lab6_set_priority]  //设置优先级
    [SYS_sleep]         //睡眠
    [SYS_open]          //打开文件
    [SYS_close]         //关闭文件
    [SYS_read]          //读输入
    [SYS_write]         //写输出
    [SYS_seek]          //查找
    [SYS_fstat]         //查询文件信息
    [SYS_fsync]         //将缓存协会磁盘
    [SYS_getcwd]        //获得当前工作目录
    [SYS_getdirentry]   //获得文件描述符对应的目录信息
    [SYS_dup]           //复制文件描述符

 1. 以ucore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。
   
  在syscall()函数中加一个输出
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
 

## v9-cpu相关题目
---

### 提前准备
```
cd YOUR v9-cpu DIR
git pull 
cd YOUR os_course_spoc_exercise DIR
git pull 
```

### v9-cpu系统调用实现
  1. v9-cpu中os4.c的系统调用中参数传递代码分析。
  1. v9-cpu中os4.c的系统调用中返回结果的传递代码分析。
  1. 理解v9-cpu中os4.c的系统调用编写和含义。

