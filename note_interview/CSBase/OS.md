# 操作系统

[TOC]

## 一、基础

### 1.1 操作系统是什么

1. 操作系统是管理计算机硬件与软件资源的程序，是计算机的基石。 
2. 操作系统本质上是⼀个运行在计算机上的软件程序 ，用于管理计算机硬件和软件资源。 
3. 操作系统存在屏蔽了硬件层的复杂性。
4. 操作系统的内核（Kernel）是操作系统的核心部分，它负责系统的内存管理，硬件设备的管 理，文件系统的管理以及应用程序的管理。 内核是连接应用程序和硬件的桥梁，决定着系统的性能和稳定性。

### 1.2 系统调用

介绍系统调用之前，我们先来了解⼀下用户态和核心态。 根据进程访问资源的特点，我们可以把进程在系统上的运行分为两个级别： 

1. 用户态(user mode) : 用户态运行的进程可以直接读取用户程序的数据。 
2. 核心态(kernel mode)：可以简单的理解系统态运行的进程或程序几乎可以访问计算机的任何资源，不受限制。 

说了用户态和核心态之后，那么什么是系统调用呢？ 

系统调用（System Call）是操作系统提供给用户程序的一组接口，用于实现用户态程序对系统级资源的调用。



## 二、进程与线程

### 2.1 进程与线程

**进程**

进程是程序的一次执行过程，因此进程是动态的；进程是操作系统进行资源分配的基本单位，每个进程都有自己的独立内存空间。

**线程**

线程又叫做轻量级进程，是处理器任务调度和执行的基本单位。

线程只拥有一点在运行中必不可少的资源(如程序计数器，一组寄存器和栈)，但是它可与同属一个进程的其他的线程共享进程所拥有的全部资源。

**区别**

- 根本区别：进程是系统**资源分配**的基本单位，而线程是**处理器任务调度**和执行的基本单位。
- 内存分配：同一进程的线程共享本进程的地址空间和资源，而进程之间的地址空间和资源是相互独立的。
- 资源开销：每个进程都有独立的代码和数据空间，程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一进程的线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器，线程之间切换的开销小。

### 2.2 进程状态

⼀般把进程⼤致分为 5 种状态

+ 创建状态(new) ：进程正在被创建，尚未到就绪状态。 
+ 就绪状态(ready) ：进程已处于准备运行状态，即进程获得了除了处理器之外的⼀切所需资源， 一旦得到处理器资源(处理器分配的时间片)即可运行。 
+ 运行状态(running) ：进程正在处理器上上运行(单核 CPU 下任意时刻只有一个进程处于运行状态)。 
+ 阻塞状态(waiting) ：又称为等待状态，进程正在等待某一事件而暂停运行如等待某资源为可用 或等待 IO 操作完成。即使处理器空闲，该进程也不能运行。 
+ 结束状态(terminated) ：进程正在从系统中消失。可能是进程正常结束或其他原因中断退出运行。

### 2.3 进程的调度算法

我记得几种常见的调度算法

+ 先来先服务
+ 短作业优先
+ 优先级调度
+ 时间片轮转
+ 多级反馈队列调度算法：多级反馈队列调度算法的核心思想是通过多个队列来管理不同优先级的进程，并根据进程的行为（例如运行时间和等待时间）动态调整其优先级。不同的队列有不同的时间片（时间量），通常，较高优先级的队列具有较短的时间片，而较低优先级的队列具有较长的时间片。

### 2.4 进程间的通信方式

1. 管道
2. 信号：比如 kill -9 1050 就表示给PID为1050的进程发送 SIGKIL 信号。
3. 消息队列：消息队列就是保存在内核中的消息链表。
4. 共享内存
5. 信号量：可以用来控制多个进程对共享资源的访问。
6. Socket：与其他通信机制不同的是，它可用于不同机器间的进程通信。

### 2.5 线程间的同步

同步解决的多线程操作共享资源的问题，目的是不管线程之间的执行如何穿插，最 后的结果都是正确的。

临界区：我们把对共享资源访问的程序片段称为 临界区 ，我们希望这段代码是 互 斥 的，保证在某时刻只能被一个线程执行，也就是说一个线程在临界区执行时，其 它线程应该被阻止进入临界区。

临界区同步的一些实现方式：

1. **锁**

使用加锁操作和解锁操作可以解决并发线程/进程的互斥问题。 任何想进入临界区的线程，必须先执行加锁操作。若加锁操作顺利通过，则线程可进入临界区；在完成对临界资源的访问后再执行解锁操作，以释放该临界资源。 加锁和解锁锁住的是什么呢？可以是临界区对象，也可以只是一个简单的**互斥量** 。

比如 Java 中的 synchronized 关键词和各种 Lock 都是这种机制。

2. **信号量**

信号量是操作系统提供的一种协调共享资源访问的方法。

通常信号量表示资源的数量，对应的变量是⼀个整型变量。 通过 **P** 和 **V** 两个原子操作的系统调用函数来控制信号量。

### 2.6 死锁

在并发线程中，多个线程互相等待被别的线程所持有的临界区资源。

**四个必要条件**

1. 互斥条件：共享资源互斥访问；
2. 持有并等待：线程已经持有的资源 1，还在等待资源 2 ；
3. 不可剥夺：线程在自己使用完资源之前该资源不能被其他线程获取；
4. 循环等待链：多个线程获取资源的顺序构成了环形链；

**如何避免**

互斥条件不可破坏

一次性分配完所需资源。

剥夺已分配资源，造成饥饿，增加线程切换的带来的上下文切换的损耗。

资源有序分配



## 三、内存管理

### 3.1 虚拟内存

虚拟内存允许我们的操作系统为每个运行的程序分配一个远大于物理内存（RAM）的虚拟地址空间。这种技术通过结合物理内存和硬盘上的存储空间来工作，从而有效地扩展了系统的可用内存。

虚拟内存为每个进程提供了一段连续的虚拟地址空间，可以更加有效的管理内存，避免进程间的相互修改内存数据。

### 3.2 内存管理方式

简单分为**连续分配管理方式**和**非连续分配管理方式**这两种。

连续分配管理方式是指为一个用户程序分配一个连续的内存空间，常见的如 **块式管理** 。

非连续分配管理方式允许一个程序使用的内存分布在离散或者说不相邻的内存中，常见的如**页式管理** 和 **段式管理**

1. **块式管理** ： 远古时代的计算机操作系统的内存管理方式。将内存分为几个固定大小的块，每个块中只包含一个进程。如果程序运行只需要很小的空间的话，分配的这块内存很大一部分几乎被浪费了。
2. **页式管理** ：把主存分为大小相等且固定的一页一页的形式，页较小，相比于块式管理的划分粒度更小，提高了内存利用率，减少了碎片。页式管理通过页表对应逻辑地址和物理地址。
3. **段式管理** ： 页式管理虽然提高了内存利用率，但是页式管理其中的页并无任何实际意义。 段式管理把主存分为一段段的，段是有实际意义的，每个段定义了一组逻辑信息，例如,有主程序段 MAIN、子程序段 X、数据段 D 及栈段 S 等。 段式管理通过段表对应逻辑地址和物理地址。
4. **段页式管理**：段页式管理机制结合了段式管理和页式管理的优点。简单来说段页式管理机制就是把主存先分成若干段，每个段又分成若干页，也就是说 段页式管理机制 中段与段之间以及段的内部的都是离散的。

### 3.3 快表 和 多级页表

在分页内存管理中，很重要的两点是： 

1. 虚拟地址到物理地址的转换要快。 
2. 解决虚拟地址空间大，页表也会很大的问题。

**快表**

为了提高虚拟地址到物理地址的转换速度，操作系统在页表方案基础之上引入了 快表 来加速虚拟 地址到物理地址的转换。快表其实就是页表的一种高速缓冲存储器（Cache）。由于采用页表做地址转换，读写内存数据时 CPU 要访问两次主存。有了快表，有时只要访问一次高速缓存，一次主存，这样可加速查找并提高指令执行速度。

**多级页表**

所谓的多级页表，就是把我们原来的单级页表再次分页，这里利用了 **局部性原理** ， 除了顶级页表，其它级别的页表一来可以在需要的时候才被创建，二来内存紧张的 时候还可以被置换到磁盘中。

快表利用了 时间局部性 原理，多级页表利用了空间局部性原理。

### 3.4 局部性原理

1. 时间局部性：如果程序中的某条指令一旦执行，不久以后该指令可能再次执行；如果某数据被访问过，不久以后该数据可能再次被访问。产生时间局部性的典型原因，是由于在程序中存在着大龄的循环操作。
2. 空间局部性：一旦程序访问了某个存储单元，在不久之后，其附近的存储单元也将被访问，即程序在一段时间内所访问的地址，可能集中在一定的范围之内，这是因为指令通常是顺序存放、 顺序执行的，数据也一般是以向量、数组、表等形式簇聚存储的。

### 3.5 虚拟内存的技术实现

虚拟内存的实现需要建立在离散分配的内存管理方式的基础上。 虚拟内存的实现有以下三种方式：

1. **请求分页存储管理** ：建立在分页管理之上，为了支持虚拟存储器功能而增加了请求调页功能和页面置换功能。
2. **请求分段存储管理** ：建立在分段存储管理之上，增加了请求调段功能、分段置换功能。
3. **请求段页式存储管理**

### 3.6 页面置换算法

1. OPT（最佳页面置换算法） ：最佳置换算法所选择的被淘汰页面将是以后永不使用的，或者是在最长时间内不再被访问的页面,这样可以保证获得最低的缺页率。但是该算法目前无法实现。一般作为衡量其他置换算法的方法。 
2. FIFO（先进先出页面置换算法） : 总是淘汰最先进入内存的页面，即选择在内存中驻留时间最久的页面进行淘汰。 
3. LRU（最近最久未使用页面置换算法）：最近最久未使用的页面淘汰。 
4. LFU（最少使用页面置换算法）: 该置换算法选择在之前时期使用最少的页面作为淘汰页。



## 四、文件

### 4.1 硬链接和软连接

+ **硬链接**就是在目录下创建一个条目，记录着文件名与 inode 编号，这个 inode 就是源文件的 inode。删除任意一个条目，文件还是存在，只要引用数量不为 0。但是硬链接有限制，它不能跨越文件系统，也不能对目录进行链接。

![image-20240812164621833](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121646058.png)

+ 软链接相当于重新创建一个文件，这个文件有独立的 inode，但是这个文件的内容是另外一个文件的路径，所以访问软链接的时候，实际上相当于访问到了另外一个文件，所以软链接是可以跨文件系统的，甚至目标文件被删除了， 链接文件还是在的，只不过打不开指向的文件了而已。

![image-20240812164835943](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121648045.png)



## 五、IO

### 5.1 零拷贝

传统I/O，数据读取和写入是用户空间到内核空间来回赋值，而内核空间的数据是通过操作系统的I/O接口从磁盘或者网络来读取或者写入的，这期间发生了多次用户态和内核态的上下文切换，以及多次数据拷贝，效率很低。

![image-20240812172941736](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121729827.png)

为了提升I/O性能，就需要**减少用户态与内核态的上下文切换**和**内存拷贝的次数**。

两种实现方式：

+ mmap + write

mmap() 系统调用函数会直接把内核缓冲区里的数据「映射」到用户空间，这样，操作系统内核与用户空间就不需要再进行任何的数据拷贝操作。

![image-20240812173114304](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121731381.png)

+ sendfile

在 Linux 内核版本 2.1 中，提供了一个专门发送文件的系统调用函数 sendfile() 。 

首先，它可以替代前面的 read() 和 write() 这两个系统调用，这样就可以减少一次系统调用，也就减少了 2 次上下文切换的开销。 其次，该系统调用，可以直接把内核缓冲区里的数据拷贝到 socket 缓冲区里，不再拷贝到用户态。

![image-20240812173726760](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121737855.png)

### 5.2 BIO、NIO、AIO和IO多路复用

+ **BIO（阻塞IO）**

当用户程序执行 read ，线程会被阻塞，一直等到内核数据准备好，并把数据从内核缓冲区拷贝到应用程序的缓冲区中，当拷贝过程完成， read 才会返回。

注意，阻塞IO等待的是 **内核数据准备** 和 **数据从内核态拷贝到用户态** 这两个过程。

![image-20240812180911506](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121809611.png)

+ **NIO（非阻塞IO）**

非阻塞的 read 请求在数据未准备好的情况下立即返回，可以继续往下执行，此时应用程序可以不断轮询内核，直到数据准备好，内核将数据拷贝到应用程序缓冲区，read 调用才可以获取到结果。

![image-20240812181401576](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121814718.png)

+ **IO多路复用**

非阻塞I/O有一个问题，应用程序要一直轮询，这个过程没法干其它事情，所以引入了I/O 多路复用技术。 当内核数据准备好时，以事件通知应用程序进行操作。

![image-20240812182342125](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121823237.png)

+ **AIO（异步IO）**

真正的异步 I/O 是 内核数据准备好 和 数据从内核态拷贝到用户态 这两个过程都不用等待。 

发起 aio_read 之后，就立即返回，内核自动将数据从内核空间拷贝到应用程序空间，这个拷贝过程同样是异步的，内核自动完成的，和前面的同步操作不⼀样，应用程序并不需要主动发起拷贝动作。

![image-20240812182529451](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121825534.png)

### 5.3 IO多路复用

如果服务端需要支持多个客户端，我们可能要为每个客户端分配一个进程/线程。 假如连接多了，操作系统是扛不住的。 所以就引入了I/O多路复用技术。

简单说，就是一个进程/线程维护多个Socket，这个多路复用就是多个连接复用一个 进程/线程

三种实现方式：

+ **select**

将已连接的 Socket 都放到一个文件描述符集合 fd_set，然后调用 select 函数将fd_set 集合拷贝到内核里，让内核来检查是否有网络事件产生，当检查到有事件发生后，将此 Socket 标记为可读或可写， 接着再把整个fd_set拷贝回用户态里，然后用户态还需要再通过遍历的方法找到可读 或可写的 Socket，再对其处理。

select 使用固定长度的 BitsMap，表示文件描述符集合。

**缺点**：

1. 每次调用select，都需要把fd_set集合从用户态拷贝到内核态
2. fd_set集合固定大小
3. 想找到可读的 Socket 需要遍历

+ **poll**

poll 不再用 BitsMap 来存储所关注的文件描述符，取而代之用动态数组，以链表形式来组织，突破了 select 的文件描述符个数限制。 但是 poll 和 select 并没有太大的本质区别，都是使用线性结构存储进程关注的Socket 集合，因此都需要遍历文件描述符集合来找到可读或可写的Socke，时间复杂度为 O(n)，而且也需要在用户态与内核态之间拷贝文件描述符集合，这种方式随着并发 数上来，性能的损耗会呈指数级增长。

+ **epoll**

epoll 通过两个方法，很好解决了 select/poll 的问题。 

第一：epoll 在内核里使用红黑树来跟踪进程所有待检测的文件描述符，把需要监控的 socket 通过epoll_ctl() 函数加入内核中的红黑树里，红黑树是个高效的数据结构，增删查一般时间复杂度是O(logn) ，通过对这棵黑红树进行操作，这样就不需要像 select/poll 每次操作时都传入整个 socket 集合，只需要传入一个待检测的 socket，减少了内核和用户空间大量的数据拷贝和内存分配。 

第⼆点， epoll 使用事件驱动的机制，内核里维护了一个链表来记录就绪事件，当某个 socket 有事件发生时，通过回调函数，内核会将其加入到这个就绪事件列表中，当用户调用 epoll_wait() 函数时，只会返回有事件发生的文件描述符的个数，不需要像 select/poll 那样轮询扫描整个 socket 集合，大大提高了检测的效率。

![image-20240812184453251](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408121844328.png)