
## 内存管理
* [20 张图揭开内存管理的迷雾！](https://mp.weixin.qq.com/s/4FF5uH0YVTAM9-llKTAWKA)
  * 虚拟内存的目的是为了在多进程环境中，每个进程之间的内存地址不受影响。否则，大家都直接去访问物理地址，就会导致错误和安全问题。
  * 当很多进程存在时，物理内存吃紧。所以操作系统会做**内存交换**，把不常用的内存暂时放到硬盘里 (swap out)，在需要时重新放回物理内存 (swap in).
  * 从虚拟地址到物理地址的映射有两种方法：*分段 (segmentation)* 和 *分页 (paging)*
  * 首先是*分段*，一个进程包括代码段、数据段、堆、栈等。使用 *segment selector* 和 *offset* 就能确定物理地址。但是，会导致两个问题：
    * 内存碎片。多进程中，新进程能分配的物理地址可能不连续。可以将某些进程的内存暂时放回硬盘里（swap out），拷贝回物理内存时，放到另一个位置，腾出连续的空间。
    * 内存交换的效率低。因为硬盘速度太慢，拷贝整个进程的内存太浪费时间。
  * 所以引入了*分页*，把虚拟内存和物理内存分成大小固定的页，比如Linux中的4KB。这样避免了细小的内存碎片，同时*内存交换*时，只需要与硬盘交换一个或几个页，大大提高了效率。
  * 但是，需要消耗大量内存来存储页表，所以引入了*多级页表*。它解决了空间上的问题，但这就会导致 CPU 在寻址的过程中，需要有很多层表参与，加大了时间上的开销。于是根据程序的*局部性原理*，在 CPU 芯片中加入了 TLB （Translation Lookaside Buffer），负责缓存最近常被访问的页表项，大大提高了地址的转换速度。
  * 分段和分页是可以结合的。
  * Linux系统主要采用了分页管理。但由于 Intel 处理器的发展史，Linux 系统无法避免分段管理。于是 Linux 就把所有段的基地址设为 0，也就意味着所有程序的地址空间都是线性地址空间（虚拟地址），相当于屏蔽了 CPU 逻辑地址的概念，所以段只被用于访问控制和内存保护。另外，Linxu 系统中虚拟空间分布可分为*用户态*和*内核态*两部分，其中用户态的分布：代码段、全局变量、BSS、函数栈、堆内存、映射区。
* [Paging and CR3](https://cirosantilli.com/x86-paging).

## Ring0 and Ring3
* [What are Ring 0 and Ring 3 in the context of operating systems?](https://stackoverflow.com/questions/18717016/what-are-ring-0-and-ring-3-in-the-context-of-operating-systems)
  * Linux uses Ring 0 for kernel and Ring 3 for users.
  * Ring 0 can do anything.
  * Ring 3 cannot do several things:
    * cannot change its own ring.
    * cannot modify the page tables (See above about paging). In other words, cannot modify CR3 register to see the memory of other processes.
    * cannot register interrupt handlers. Handlers need to run in ring 0, so ring 3 cannot register handlers.
    * cannot do IO instructions like `in` and `out`, and thus have arbitrary hardware accesses.


## Linux 零拷贝Zero-Copy技术
* [图解|零拷贝Zero-Copy技术大揭秘](https://mp.weixin.qq.com/s?__biz=MzI1MzYzMTI2Ng==&mid=2247485381&idx=1&sn=b2a702abc8e82f6e3b73952db1b6c5a4&chksm=e9d0c988dea7409e5c2188cfbaa207cd255f6156ac12d40e44f2d5ef160ae0beb835f9586ba6&scene=132#wechat_redirect)。里面的图非常精彩，比文字清晰很多。
  * Linux系统中一切皆文件，很多活动无外乎**读操作**和**写操作**。
  * 常见的用途是从磁盘读取数据放到网卡传输。这其中涉及很多方式和步骤。
  * 只用CPU的方式：
    * 应用程序调用`read()`，然后从用户态切换到内核态；
    * CPU向磁盘发起I/O请求，磁盘收到之后开始准备数据；
    * 磁盘将数据放到磁盘缓冲区之后，向CPU发起I/O中断，报告CPU数据已经Ready了；
    * CPU收到磁盘控制器的I/O中断之后，开始从磁盘缓冲区拷贝数据到内核缓冲区，完成之后`read()`返回，再从内核态切换到用户态；
    * CPU把数据从内核缓冲区拷贝到用户缓冲区。
    * 可以看到，这里一共发生了两次空间转换，两次CPU拷贝。
  * 用CPU + DMA的方式：
    * DMA（Direct Memory Access）是直接内存访问，是一种硬件设备，减轻了CPU的压力。目前支持DMA的硬件包括：网卡、声卡、显卡、磁盘控制器等。
    * 由 DMA 来向磁盘发起I/O请求并从磁盘缓冲区拷贝到内核缓冲区。
  * 无论从仅CPU方式和DMA&CPU方式，都存在多次冗余数据拷贝和内核态&用户态的切换。
  * 零拷贝技术：如果应用程序不对数据做修改，那么就不需要将数据从内核缓冲区拷贝到用户缓冲区，然后又拷贝到内核的套接字缓冲区。这两次拷贝可以省略。
  * 零拷贝技术的几个实现手段包括：mmap+write、sendfile、sendfile+DMA收集、splice等。
  * mmap的方式可以从内核缓冲区用CPU拷贝到套接字缓冲区。此时用户缓冲区和内核缓冲区共享一块内存，所以可以修改数据。
  * sendfile方式是直接用CPU从内核缓冲区拷贝到套接字缓冲区，不跟用户缓冲区共享，所以不能修改数据。
  * sendfile+DMA方式可以只将元数据用CPU拷贝到套接字缓冲区，剩余内容由DMA拷贝到网卡，节约了CPU资源。
  * splice方式是建立了管道，内容直接由DMA拷贝到网卡。完全避免了CPU的拷贝。它也有一些局限，比如两个文件描述符参数中有一个必须是管道设备。