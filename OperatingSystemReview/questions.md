#### 1. Introduction

-  system call 和 ISR 的映射关系是怎样的，system call 也会唤醒ISR 吗， ISR 是怎样被唤醒的， 怎样产生interrupt number的， 软件是怎么产生interrupt number ， 这个number 又是怎么传过去的呢？
  - 在ISR 执行过程中，interrupt是被屏蔽了的，所以会导致很长的system call处理时间的错觉，然而Linux并没有（至少早期版本）软件 interrupt handler。 接下来讲system call 触发ISR的具体机制
  - [system call](http://www.linux.it/~rubini/docs/ksys/)函数的作用通常是把数据存储在进程内而不是磁盘上，此后用 int 0X80 将控制转移给kernel，具体的system call 被拷贝到EAX register中，然后类似于syscall_tab[EAX] ()来执行相应的函数
  - 上面讲到了ISR执行过程中interrupt被屏蔽vi，这是不完整的，通常hardware interrupt 会block其他hardware interrupt， 但是 似乎并不会block trap. [trap and interrupt](https://www.quora.com/What-is-the-difference-between-Trap-and-Interrupt)
- exception和interrupt 有什么区别呢？exception的OS 处理机制又是怎样的呢？
  -  [definition of trap](https://en.wikipedia.org/wiki/Trap), exception 和 trap 在大多数情况下同理，类似的还有fault

#### 2. Process and threads

-  Program Counter 是独立硬件，还是存储在register set中的？
  -  这个问题不好回答，不同架构芯片对这个处理不一样，只要知道program counter是一个register就好了，可认为包含在register set 中
-  waiting queue 和 ready queue的具体表现区别在哪里？
   -  <Question>对于多核来说，waiting state 还没有被分配具体的CPU, 那么这个时候问题来了, waiting state有PCB吗？waiting state 有其他的东西吗？代码块被加载到memory了吗？还是说连代码块都还没有被加载？这里涉及到一部分virutal memory的东西？
   -  ready queue是在main memory中的，等待被执行
   -  <Question>wait queue : 每个 wait queue 在linux中都会有一个spinlock，理论上应该是在kernel space的，但是不清楚此时的code section在哪里？我认为应该也在内存中，只是PCB在wait queue而已，而PCB存储了CPU作 context switch所需的几乎所有信息。wait queue，比如某个process需要一个disk中的block但是还没有被fetch，这个时候就需要被放到wait queue。不过这时候又有一个问题了，这种算是I/O queue呢还是wait queue? 又是谁去执行这个拿block的操作呢？拿完之后会被立即放回ready queue还是按照顺序呢（这个应该和long term scheduler 策略有关），什么情况下ready queue中的process会被放回wait queue 呢 （swap midterm scheduler?）
-  PCB 在哪些地方会出现？
   -  <Question> ready queue / wait queue /
-  什么又是 I/O queue? 一个程序在I/O queue中也一定在wait queue中吗？
   -  I/O queue 是一种可以调度I/O 运行的组件，非常详细的介绍在这里[Linux I/O 调度器](https://www.ibm.com/developerworks/cn/linux/l-lo-io-scheduler-optimize-performance/index.html) 
-  什么是VFS
   -  virtual file system
-  多线程的最小执行单位是thread，那为什么没有thread control block 而是只有 PCB 呢？ 如果运行一个多线程process，有可能同一个process的线程在不同的处理器上跑吗？还是说其实还有一个专门管理线程的queue？
-  process 的wait() 的参数是什么意思
   -  如果单纯NULL，就是等待任意child执行结束，如果传入一个&status的指针，就是还想要这个子线程是否返回成功的一些信息。
-  如何避免Zombie Process 以及Orphan Process 
-  threads 间如何share message, 谁负责管理奋发signals
   -  [Linux signals](https://www.cnblogs.com/taobataoma/archive/2007/08/30/875743.html) 是一种，当然还有condition variables 都是可以的， 但是其实有很多很多的实现方法，pipe也好信箱也好，其实都是交流的一种方式，具体的东西会在之后的并发编程中遇到。

#### 3.  CPU Scheduling

- I/O queue 的位置在哪里呢？kenel memory还是I/O 控制器自定义的某个地方？
- 这里为什么又有job会到disk中，连读都不读了吗
- 什么东西把process从disk中读到memory中呢？添加PCB的操作是谁做的?
- 当一个被preempt的process从ready queue中退出的时候，会被放到哪里去？放到哪里去包含两部分，一部分是process本体，一部分是PCB，同理还有wait queue， I/O queue的相关东西
- SJF 的 CPU burst prediction是指对单个process不同burst的猜测吗？

#### 4. IPC and synchronization

- send 是system call 吗？ 执行一个 send receive 一共会有几个kernel 到 user space的转换
- 所有会导致 user kernel mode 转换的情况有哪些？
- 为什么Perterson的method会把其他的设置成true
- TODO ： implement all methods in synchronization 并且可以再看一遍书
- 为什么semaphore的wait 和 signal 要disable interrupts in uniprocessor system
- TODO : 背一遍所有的impelmentation
  - test_and_Set, compare_and_swap, Peterson's, 3 problem and DP using monitors.
  - some other not so important ones

#### 6. Memory Management

- page table 在哪里？kernel还是user space
  - 有的会把页表存储在virtual memory中
  - 但是通常来讲会在kernel memory中，不过随着页表变大，部分会存在disk中，我猜是swap space
- 为什么是在same logical address而不是 physical address
- TODO : 学会计算页表的大小， Effective Access Time
- 如何使用Hash Table 来implement inverted Page Table
- PAE 是什么东西

#### 7. Virtual Memory

- 如何确定一个标志位是i的page table entry 是 invalid memory access 还是 page fault?
- TODO : Memory Access 次数的计算
- 为啥Minor Page  Fault里面可以有动态的allocate physical ？
  - 可能直到那块memory真正被用到的时候才给，并不是在malloc的时候分配，不过这个为啥就是minor了？
- TODO ： virtual page replacement algorithms



