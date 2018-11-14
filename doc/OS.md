## 操作系统主要知识点

[下面部分来自CyC2018](https://github.com/CyC2018/Interview-Notebook/blob/master/notes/%E8%AE%A1%E7%AE%97%E6%9C%BA%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F.md)

### 1.基本特征

1. 并发
并发是指宏观上在一段时间内能同时运行多个程序，而并行则指同一时刻能运行多个指令。

并行需要硬件支持，如多流水线或者多处理器。

操作系统通过引入进程和线程，使得程序能够并发运行。

2. 共享
共享是指系统中的资源可以被多个并发进程共同使用。

有两种共享方式：互斥共享和同时共享。

互斥共享的资源称为临界资源，例如打印机等，在同一时间只允许一个进程访问，需要用同步机制来实现对临界资源的访问。

3. 虚拟
虚拟技术把一个物理实体转换为多个逻辑实体。

主要有两种虚拟技术：时分复用技术和空分复用技术。例如多个进程能在同一个处理器上并发执行使用了时分复用技术，让每个进程轮流占有处理器，每次只执行一小个时间片并快速切换。

4. 异步
异步指进程不是一次性执行完毕，而是走走停停，以不可知的速度向前推进。

### 2.基本功能
1. 进程管理
进程控制、进程同步、进程通信、死锁处理、处理机调度等。

2. 内存管理
内存分配、地址映射、内存保护与共享、虚拟内存等。

3. 文件管理
文件存储空间的管理、目录管理、文件读写管理和保护等。

4. 设备管理
完成用户的 I/O 请求，方便用户使用各种设备，并提高设备的利用率。

主要包括缓冲管理、设备分配、设备处理、虛拟设备等。

### 3.大内核和微内核

1. 大内核
大内核是将操作系统功能作为一个紧密结合的整体放到内核。

由于各模块共享信息，因此有很高的性能。

2. 微内核
由于操作系统不断复杂，因此将一部分操作系统功能移出内核，从而降低内核的复杂性。移出的部分根据分层的原则划分成若干服务，相互独立。

在微内核结构下，操作系统被划分成小的、定义良好的模块，只有微内核这一个模块运行在内核态，其余模块运行在用户态。

因为需要频繁地在用户态和核心态之间进行切换，所以会有一定的性能损失。

## 4.关于中断

什么是中断？

中断是指CPU对I/O设备发来的中断信号的一种响应。CPU暂停正在执行的程序，保留CPU环境后，自动地去执行该I/O设备的中断处理程序。执行完后，再回到断点，继续执行原来的程序。I/O设备可以是字符设备，也可以是块设备、通信设备。由于中断时由外部设备引起的，故又称外中断。

硬件中断时通过中断请求线输入信号来请求处理机；软件中断是处理机内部识别并进行处理的中断过程。硬件中断一般是由中断控制器提供中断码类型，处理机自动转向中断处理程序；软件中断完全有处理机内部形成中断处理程序的入口地址并转向中断处理程序的入口地址，并转向中断处理程序，不需要外部提供信息。

1. 外中断
由 CPU 执行指令以外的事件引起，如 I/O 完成中断，表示设备输入/输出处理已经完成，处理器能够发送下一个输入/输出请求。此外还有时钟中断、控制台中断等。

2. 异常
由 CPU 执行指令的内部事件引起，如非法操作码、地址越界、算术溢出等。

3. 陷入
另外一种由CPU内部事件所引起的中断，例如进程在运算中发生了上溢或者下溢，有如程序出错，如非法指令，地址越界等。通常把这类中断称为内中断或者陷入。若系统发现有陷入事件，CPU也将暂停正在执行的程序，转去执行该陷入事件的处理程序。中断和陷入的主要区别是信号的来源，是来自CPU外部，还是CPU内部。

中断处理流程？

1. 测定是否有未响应的中断信号。
2. 保护被中断进程的CPU环境。
3. 转入相应的设备处理程序。
4. 中断处理。
5. 恢复CPU的现场并退出中断
中断的实时性是实时系统的一个重要方面。中断响应时间是影响中断实时性的主要因素。

### 5.进程与线程有什么区别？

1. 进程
进程是资源分配的基本单位。
进程控制块 (Process Control Block， PCB) 描述进程的基本信息和运行状态，所谓的创建进程和撤销进程，都是指对 PCB 的操作。

2. 线程
线程是独立调度的基本单位。
一个进程中可以有多个线程，它们共享进程资源。线程的划分尺度小于进程，使得多线程程序的并发性高。
QQ 和浏览器是两个进程，浏览器进程里面有很多线程，例如 HTTP 请求线程、事件响应线程、渲染线程等等，线程的并发执行使得在浏览器中点击一个新链接从而发起 HTTP 请求时，浏览器还可以响应用户的其它事件。

区别 

（一）拥有资源

进程是资源分配的基本单位，但是线程不拥有资源，线程可以访问隶属进程的资源。

（二）调度

线程是独立调度的基本单位，在同一进程中，线程的切换不会引起进程切换，从一个进程内的线程切换到另一个进程中的线程时，会引起进程切换。

（三）系统开销

由于创建或撤销进程时，系统都要为之分配或回收资源，如内存空间、I/O 设备等，所付出的开销远大于创建或撤销线程时的开销。类似地，在进行进程切换时，涉及当前执行进程 CPU 环境的保存及新调度进程 CPU 环境的设置，而线程切换时只需保存和设置少量寄存器内容，开销很小。

（四）通信方面

进程间通信 (IPC) 需要进程同步和互斥手段的辅助，以保证数据的一致性。而线程间可以通过直接读/写同一进程中的数据段（如全局变量）来进行通信。

3. 什么是协程？作用是什么？

协程，英文Coroutines，是一种比线程更加轻量级的存在。正如一个进程可以拥有多个线程一样，一个线程也可以拥有多个协程。
最重要的是，协程不是被操作系统内核所管理，而完全是由程序所控制（也就是在用户态执行）。
这样带来的好处就是性能得到了很大的提升，不会像线程切换那样消耗资源。其本质是用户空间下的线程。（即不存在上下文切换，用户与内核地址空间切换，但协程是在单线程中运行）

4. 协程的应用有哪些？

Lua从5.0版本开始使用协程，通过扩展库coroutine来实现。

Python可以通过 yield/send 的方式实现协程。在python 3.5以后，async/await 成为了更好的替代方案。

Go语言对协程的实现非常强大而简洁，可以轻松创建成百上千个协程并发执行。

Java语言并没有对协程的原生支持，但是某些开源框架模拟出了协程的功能，有兴趣的小伙伴可以看一看Kilim框架的源码：https://github.com/kilim/kilim

补充

1. 历史上是先有协程，是OS用来模拟多任务并发
2. 线程确实比协程性能更好
3. 说协程性能好的，其实真正的原因是因为瓶颈在IO上面
4. 协程的确可以减少callback的使用但是不能完全替换callback

下面这个很详细，而且观点也很多，直接贴链接<br>
[知乎- 协程的好处有哪些？](https://www.zhihu.com/question/20511233)

### 6.进程有哪几种状态？

一般来说，进程有三个状态，即就绪状态，运行状态，阻塞状态。
	
	   运行态：进程占用CPU，并在CPU上运行；
	   就绪态：进程已经具备运行条件，但是CPU还没有分配过来；
	   阻塞态：进程因等待某件事发生而暂时不能运行；
	   
当然理论上上述三种状态之间转换分为六种情况；

* 运行——>就绪：1，主要是进程占用CPU的时间过长，而系统分配给该进程占用CPU的时间是有限的；2，在采用抢先式优先级调度算法的系统中，当有更高优先级的进程要运行时，该进程就被迫让出CPU，该进程便由执行状态转变为就绪状态。
* 就绪——>运行：运行的进程的时间片用完，调度就转到就绪队列中选择合适的进程分配CPU
* 运行——>阻塞：正在执行的进程因发生某等待事件而无法执行，则进程由执行状态变为阻塞状态，如发生了I/O请求
* 阻塞——>就绪:进程所等待的事件已经发生，就进入就绪队列

阻塞状态是缺少需要的资源从而由运行状态转换而来，但是该资源不包括 CPU 时间，缺少 CPU 时间会从运行态转换为就绪态。

以下两种状态是不可能发生的：

* 阻塞——>运行：即使给阻塞进程分配CPU，也无法执行，操作系统在进行调度时不会从阻塞队列进行挑选，而是从就绪队列中选取
* 就绪——>阻塞：就绪态根本就没有执行，谈不上进入阻塞态。

在不少系统中进程只有上述三种状态，但在另一些系统中，又增加了一些新状态，最重要的是挂起状态。引入挂起状态的原因有：

1. 终端用户的请求。当终端用户在自己的程序运行期间发现有可疑问题时，希望暂时使自己的程序静止下来。亦即，使正在执行的进程暂停执行；若此时用户进程正处于就绪状态而未执行，则该进程暂不接受调度，以便用户研究其执行情况或对程序进行修改。我们把这种静止状态称为挂起状态。
2. 父进程请求。有时父进程希望挂起自己的某个子进程，以便考查和修改该子进程，或者协调各子进程间的活动。
3. 负荷调节的需要。当实时系统中的工作负荷较重，已可能影响到对实时任务的控制时，可由系统把一些不重要的进程挂起，以保证系统能正常运行。
4. 操作系统的需要。操作系统有时希望挂起某些进程，以便检查运行中的资源使用情况或进行记账。

		作者：陕西搜讯
		链接：https://www.jianshu.com/p/ac9ce2afd126
		來源：简书
		简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。

创建状态

创建一个进程一般要通过一下两个两个步骤

1. 为一个新进程创建PCB，并填写必要的管理信息.

2. 把该进程转入就绪状态并插入就绪队列之中。当一个新进程被创建时，系统已为其分配了PCB，填写了进程标识等信息，但由于该进程所必需的资源或其它信息，如主存资源尚未分配等，一般而言，此时的进程已拥有了自己PCB，但进程自身还未进入主存，即创建工作尚未完成，进程还不能被调度运行，其所处的状态就是创建状态。 引入创建状态，是为了保证进程的调度必须在创建工作完成后进行，以确保对进程控制块操作的完整性。同时，创建状态的引入，也增加了管理的灵活性，操作系统可以根据系统性能或主存容量的限制，推迟创建状态进程的提交。对于处于创建状态的进程，获得了其所必需的资源，以及对其PCB初始化工作完成后，进程状态便可由创建状态转入就绪状态。

终止状态

等待操作系统进行善后处理，然后将其PCB清零，并将PCB空间返还系统。当一个进程到达了自然结束点，或是出现了无法克服的错误，或是被操作系统所终结，或是被其他有终止权的进程所终结，它将进入终止状态。进入终止态的进程以后不能再执行，但在操作系统中依然保留一个记录，其中保存状态码和一些计时统计数据，供其它进程收集。一旦其它进程完成了对终止状态进程的信息提取之后，操作系统将删除该进程。

### 7.进程调度|批处理|空闲分区|页面置换算法有哪些？

常见的批处理作业调度算法

1. 先来先服务调度算法（FCFS）就是按照各个作业进入系统的自然次序来调度作业。这种调度算法的优点是实现简单，公平。其缺点是没有考虑到系统中各种资源的综合使用情况，往往使短作业的用户不满意，因为短作业等待处理的时间可能比实际运行时间长得多。
2. 短作业优先调度算法（SJF）就是优先调度并处理短作业，所谓短是指作业的运行时间短。而在作业未投入运行时，并不能知道它实际的运行时间的长短，因此需要用户在提交作业时同时提交作业运行时间的估计值。
3. 最高响应比优先算法（HRN）FCFS可能造成短作业用户不满，SPF可能使得长作业用户不满，于是提出HRN，选择响应比最高的作业运行。响应比=1+作业等待时间/作业处理时间。
4. 基于优先数调度算法（HPF）每一个作业规定一个表示该作业优先级别的整数，当需要将新的作业由输入井调入内存处理时，优先选择优先数最高的作业。
5. 均衡调度算法，即多级队列调度算法
6. 最短剩余时间优先（SRTN）按估计剩余时间最短的顺序进行调度。

基本概念

作业周转时间（Ti）＝完成时间（Tei）－提交时间（Tsi）<br>
作业平均周转时间（T）＝周转时间/作业个数<br>
作业带权周转时间（Wi）＝周转时间/运行时间<br>
响应比＝（等待时间＋运行时间）/运行时间<br>

进程调度算法

1. 先进先出算法（FIFO）按照进程进入就绪队列的先后次序来选择。即每当进入进程调度，总是把就绪队列的队首进程投入运行。
2. 时间片轮转算法（RR）分时系统的一种调度算法。轮转的基本思想是，将CPU的处理时间划分成一个个的时间片，就绪队列中的进程轮流运行一个时间片。当时间片结束时，就强迫进程让出CPU，该进程进入就绪队列，等待下一次调度，同时，进程调度又去选择就绪队列中的一个进程，分配给它一个时间片，以投入运行。
1. 最高优先级算法（HPF）进程调度每次将处理机分配给具有最高优先级的就绪进程。最高优先级算法可与不同的CPU方式结合形成可抢占式最高优先级算法和不可抢占式最高优先级算法。
4. 多级队列反馈法：几种调度算法的结合形式多级队列方式。

空闲分区分配算法

1. 首先适应算法：当接到内存申请时，查找分区说明表，找到第一个满足申请长度的空闲区，将其分割并分配。此算法简单，可以快速做出分配决定。
2. 最佳适应算法：当接到内存申请时，查找分区说明表，找到第一个能满足申请长度的最小空闲区，将其进行分割并分配。此算法最节约空间，因为它尽量不分割到大的空闲区，其缺点是可能会形成很多很小的空闲分区，称为“碎片”。
3. 最坏适应算法：当接到内存申请时，查找分区说明表，找到能满足申请要求的最大的空闲区。该算法的优点是避免形成碎片，而缺点是分割了大的空闲区后，在遇到较大的程序申请内存时，无法满足的可能性较大。

虚拟页式存储管理中的页面置换算法

1. 理想页面置换算法（OPT）：这是一种理想的算法，在实际中不可能实现。该算法的思想是：发生缺页时，选择以后永不使用或在最长时间内不再被访问的内存页面予以淘汰。
2. 先进先出页面置换算法（FIFO）：选择最先进入内存的页面予以淘汰。
3. 最近最久未使用算法（LRU）：选择在最近一段时间内最久没有使用过的页，把它淘汰。
4. 最少使用算法（LFU）：选择到当前时间为止被访问次数最少的页转换。

磁盘调度

1. 先来先服务（FCFS）
2. 最短寻道时间优先（SSTF）：让离当前磁道最近的请求访问者启动磁盘驱动器，即是让查找时间最短的那个作业先执行，而不考虑请求访问者到来的先后次序，这样就克服了先来先服务调度算法中磁臂移动过大的问题
3. 扫描算法（SCAN）或电梯调度算法：总是从磁臂当前位置开始，沿磁臂的移动方向去选择离当前磁臂最近的那个柱面的访问者。如果沿磁臂的方向无请求访问时，就改变磁臂的移动方向。在这种调度方法下磁臂的移动类似于电梯的调度，所以它也称为电梯调度算法。
4. 循环扫描算法（CSCAN）：循环扫描调度算法是在扫描算法的基础上改进的。磁臂改为单项移动，由外向里。当前位置开始沿磁臂的移动方向去选择离当前磁臂最近的哪个柱面的访问者。如果沿磁臂的方向无请求访问时，再回到最外，访问柱面号最小的作业请求。

### 8.进程同步有哪些方式？

1、信号量

用于进程间传递信号的一个整数值。在信号量上只有三种操作可以进行：初始化，P操作和V操作，这三种操作都是原子操作。
P操作(递减操作)可以用于阻塞一个进程，V操作(增加操作)可以用于解除阻塞一个进程。
 
 基本原理是两个或多个进程可以通过简单的信号进行合作，一个进程可以被迫在某一位置停止，直到它接收到一个特定的信号。该信号即为信号量s。
 为通过信号量s传送信号，进程可执行原语semSignal(s);为通过信号量s接收信号，进程可执行原语semWait(s);如果相应的信号仍然没有发送，则进程被阻塞，直到发送完为止。
 可把信号量视为一个具有整数值的变量，在它之上定义三个操作：
 
 * 一个信号量可以初始化为非负数
* semWait操作使信号量s减1.若值为负数，则执行semWait的进程被阻塞。否则进程继续执行。
* semSignal操作使信号量加1，若值大于或等于零，则被semWait操作阻塞的进程被解除阻塞。

2、管程

管程是由一个或多个过程、一个初始化序列和局部数据组成的软件模块，其主要特点如下：

* 局部数据变量只能被管程的过程访问，任何外部过程都不能访问。
* 一个进程通过调用管程的一个过程进入管程。
* 在任何时候，只能有一个进程在管程中执行，调用管程的任何其他进程都被阻塞，以等待管程可用。管程通过使用条件变量提供对同步的支持，这些条件变量包含在管程中，并且只有在管程中才能被访问。有两个函数可以操作条件变量：
* cwait(c)：调用进程的执行在条件c上阻塞，管程现在可被另一个进程使用。
* csignal(c)：恢复执行在cwait之后因为某些条件而阻塞的进程。如果有多个这样的进程，选择其中一个；如果没有这样的进程，什么以不做。
 

3、消息传递

消息传递的实际功能以一对原语的形式提供：

* send(destination，message)
* receive(source，message)
  
这是进程间进程消息传递所需要的最小操作集。一个进程以消息的形式给另一个指定的目标进程发送消息；进程通过执行receive原语接收消息，receive原语中指明发送消息的源进程和消息。

4、补充

PV操作由P操作原语和V操作原语组成（原语是不可中断的过程），对信号量进行操作，具体定义如下：

	P（S）：①将信号量S的值减1，即S=S-1；<br>
	                      ②如果S>=0，则该进程继续执行；否则该进程置为等待状态，排入等待队列。
	V（S）：①将信号量S的值加1，即S=S+1；<br>
	                      ②如果S>0，则该进程继续执行；否则释放队列中第一个等待信号量的进程。
	                      
PV操作的意义：我们用信号量及PV操作来实现进程的同步和互斥。PV操作属于进程的低级通信。
交互的并发进程因为他们共享资源，一个进程运行时，经常会由于自身或外界的原因而被中端，且断点是不固定的。也就是说进程执行的相对速度不能由进程自己来控制，于是就会导致并发进程在共享资源的时出现与时间有关的错误。

	  临界区 :    我们把并发进程中与共享变量有关的程序段称为临界区。
	  信号量S :  信号量的值与相应资源的使用情况有关。当它的值大于0时，表示当前可用资源的数量；
	  当它的值小于0时，其绝对值表示等待使用该资源的进程个数。
	  
进程的互斥是指当有若干个进程都要使用某一共享资源时，任何时刻最多只允许一个进程去使用该资源，其他要使用它的进程必须等待，直到该资源的占用着释放了该资源。
进程的同步是指在并发进程之间存在这一种制约关系，一个进程依赖另一个进程的消息，当一个进程没有得到另一个进程的消息时应等待，直到消息到达才被唤醒

5、经典同步问题

1. 生产者-消费者问题
2. 哲学家就餐问题
3. 读者-写者问题

6、同步机制应遵循的规则

1. 空闲让进： 
无进程处于临界区时，表名临界资源处于空闲状态，允许一个请求进入临界资源区的进程立即进入自己的临界区。 
2. 忙则等待： 
当有进程进入临界区时，表明资源正在被访问，其他想进入该临界区的资源必须等待，以保证对临界资源的互斥访问。 
3. 有限等待： 
对要求访问资源的进程，应保证在有限的时间内能进入自己的临界区，以避免陷入“死等”状态。 
4. 让权等待： 
当进程不能进入自己的临界区时，应立即释放处理机，以避免进程进入“忙等”状态。

实际上，在对临界区资源进行管理的时候，可以将标志看做一个“锁”，“锁开”进入，“锁关”等待， 起初锁是打开的。每个要进入临界资源的进程必须先对锁进程测试，当锁未开时，则必须等待，直至锁被打开。反之，当锁是打开的，则应立即把锁锁上，以阻止其他进程进入临界区。显然，测试和关锁必须是连续的，不允许分开执行。

### 9.进程通信的方式？

[以下出自博客](https://blog.csdn.net/wh_sjc/article/details/70283843)

一、管道

管道，通常指无名管道，是 UNIX 系统IPC最古老的形式。

* 它是半双工的（即数据只能在一个方向上流动），具有固定的读端和写端。
* 它只能用于具有亲缘关系的进程之间的通信（也是父子进程或者兄弟进程之间）。
* 它可以看成是一种特殊的文件，对于它的读写也可以使用普通的read、write 等函数。但是它不是普通的文件，并不属于其他任何文件系统，并且只存在于内存中。

二、FIFO

也称为命名管道，它是一种文件类型。

* FIFO可以在无关的进程之间交换数据，与无名管道不同。
* FIFO有路径名与之相关联，它以一种特殊设备文件形式存在于文件系统中。

三、消息队列

是消息的链接表，存放在内核中。一个消息队列由一个标识符（即队列ID）来标识。

* 消息队列是面向记录的，其中的消息具有特定的格式以及特定的优先级。
* 消息队列独立于发送与接收进程。进程终止时，消息队列及其内容并不会被删除。
* 消息队列可以实现消息的随机查询，消息不一定要以先进先出的次序读取，也可以按消息的类型读取。

四、信号量（semaphore）

与已经介绍过的 IPC 结构不同，它是一个计数器。信号量用于实现进程间的互斥与同步，而不是用于存储进程间通信数据。

* 信号量用于进程间同步，若要在进程间传递数据需要结合共享内存。
* 信号量基于操作系统的 PV 操作，程序对信号量的操作都是原子操作。
* 每次对信号量的 PV 操作不仅限于对信号量值加 1 或减 1，而且可以加减任意正整数。
* 支持信号量组。

五、共享内存（Shared Memory）

指两个或多个进程共享一个给定的存储区。

* 共享内存是最快的一种 IPC，因为进程是直接对内存进行存取。
* 因为多个进程可以同时操作，所以需要进行同步。
* 信号量+共享内存通常结合在一起使用，信号量用来同步对共享内存的访问。

五种通讯方式总结

1. 管道：速度慢，容量有限，只有父子进程能通讯    
2. FIFO：任何进程间都能通讯，但速度慢，去除了管道只能在父子进程中使用的限制 
3. 消息队列：容量受到系统限制，且要注意第一次读的时候，要考虑上一次没有读完数据的问题    
4. 信号量：不能传递复杂消息，只能用来同步    
5. 共享内存区：能够很容易控制容量，速度快，但要保持同步，比如一个进程在写的时候，另一个进程要注意读写的问题，相当于线程中的线程安全，当然，共享内存区同样可以用作线程间通讯，不过没这个必要，线程间本来就已经共享了同一进程内的一块内存

补：套接字

与其它通信机制不同的是，它可用于不同机器间的进程通信。

### 10.死锁产生的条件，怎么解决？

PS:数据库和操作系统本质均相同

1、产生死锁的原因主要是

1. 系统资源不足。
2. 进程运行推进的顺序不合适。
3. 资源分配不当等。

如果系统资源充足，进程的资源请求都能够得到满足，死锁出现的可能性就很低，否则就会因争夺有限的资源而陷入死锁。其次，进程运行推进顺序与速度不同，也可能产生死锁。

2、产生死锁的四个必要条件
	
	1. 互斥条件：一个资源每次只能被一个进程使用。
	2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
	3. 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
	4. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。

这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。

3、死锁的处理方法

* 死锁预防

破坏死锁的四个必要条件中的一个或多个来预防死锁。

* 鸵鸟策略

把头埋在沙子里，假装根本没发生问题。

因为解决死锁问题的代价很高，因此鸵鸟策略这种不采取任务措施的方案会获得更高的性能。当发生死锁时不会对用户造成多大影响，或发生死锁的概率很低，可以采用鸵鸟策略。
大多数操作系统，包括 Unix，Linux 和 Windows，处理死锁问题的办法仅仅是忽略它。

* 死锁检测与死锁恢复

不试图阻止死锁，而是当检测到死锁发生时，采取措施进行恢复。

（一）每种类型一个资源的死锁检测<br>
（二）每种类型多个资源的死锁检测<br>
（三）死锁恢复

	利用抢占恢复
	利用回滚恢复
	通过杀死进程恢复
	
* 死锁避免

银行家算法（Banker’s Algorithm）是一个避免死锁（Deadlock）的著名算法，是由艾兹格·迪杰斯特拉在1965年为T.H.E系统设计的一种避免死锁产生的算法。它以银行借贷系统的分配策略为基础，判断并保证系统的安全运行。

* 算法原理

我们可以把操作系统看作是银行家，操作系统管理的资源相当于银行家管理的资金，进程向操作系统请求分配资源相当于用户向银行家贷款。 
为保证资金的安全，银行家规定：
 
1. 当一个顾客对资金的最大需求量不超过银行家现有的资金时就可接纳该顾客； 
2. 顾客可以分期贷款，但贷款的总数不能超过最大需求量； 
3. 当银行家现有的资金不能满足顾客尚需的贷款数额时，对顾客的贷款可推迟支付，但总能使顾客在有限的时间里得到贷款； 
4. 当顾客得到所需的全部资金后，一定能在有限的时间里归还所有的资金. 
操作系统按照银行家制定的规则为进程分配资源，当进程首次申请资源时，要测试该进程对资源的最大需求量，如果系统现存的资源可以满足它的最大需求量则按当前的申请量分配资源，否则就推迟分配。当进程在执行中继续申请资源时，先测试该进程本次申请的资源数是否超过了该资源所剩余的总量。若超过则拒绝分配资源，若能满足则按当前的申请量分配资源，否则也要推迟分配。

### 11.硬链接和软链接 ###

软链接：
* 1.软链接，以路径的形式存在。类似于Windows操作系统中的快捷方式
* 2.软链接可以跨文件系统，硬链接不可以
* 3.软链接可以对一个不存在的文件名进行链接
* 4.软链接可以对目录进行链接

硬链接：
* 1.硬链接，以文件副本的形式存在。但不占用实际空间
* 2.不允许给目录创建硬链接
* 3.硬链接只有在同一个文件系统中才能创建

1. 第一、ln命令会保持每一处链接文件的同步性，也就是说，不论你改动了哪一处，其它的文件都会发生相同的变化；
2. 第二、ln的链接又分软链接和硬链接两种，软链接就是ln –s 源文件 目标文件，它只会在你选定的位置上生成一个文件的镜像，不会占用磁盘空间，
硬链接 ln 源文件 目标文件，没有参数-s， 它会在你选定的位置上生成一个和源文件大小相同的文件，无论是软链接还是硬链接，文件都保持同步变化。
ln指令用在链接文件或目录，如同时指定两个以上的文件或目录，且最后的目的地是一个已经存在的目录，则会把前面指定的所有文件或目录复制到该目录中。
若同时指定多个文件或目录，且最后的目的地并非是一个已存在的目录，则会出现错误信息。

### 12.BIO、NIO、AIO ###

一般来说I/O模型可以分为：同步阻塞，同步非阻塞，异步阻塞，异步非阻塞IO

BIO

* Java在JDK1.4出来之前，我们建立网络连接的时候采用BIO模式，是同步阻塞的。

NIO 

* NIO本身是基于事件驱动思想来完成的，其主要想解决的是BIO的大并发问题： 在使用同步I/O的网络应用中，如果要同时处理多个客户端请求，或是在客户端要同时和多个服务器进行通讯，就必须使用多线程来处理。
也就是说，将每一个客户端请求分配给一个线程来单独处理。这样做虽然可以达到我们的要求，但同时又会带来另外一个问题。由于每创建一个线程，就要为这个线程分配一定的内存空间（也叫工作存储器），而且操作系统本身也对线程的总数有一定的限制。
如果客户端的请求过多，服务端程序可能会因为不堪重负而拒绝客户端的请求，甚至服务器可能会因此而瘫痪。
* NIO基于Reactor，当socket有流可读或可写入socket时，操作系统会相应的通知引用程序进行处理，应用再将流读取到缓冲区或写入操作系统。
也就是说，这个时候，已经不是一个连接就要对应一个处理线程了，而是有效的请求，对应一个线程，当连接没有数据时，是没有工作线程来处理的。
 
BIO与NIO一个比较重要的不同，是我们使用BIO的时候往往会引入多线程，每个连接一个单独的线程；而NIO则是使用单线程或者只使用少量的多线程，每个连接共用一个线程。

AIO

* NIO的最重要的地方是当一个连接创建后，不需要对应一个线程，这个连接会被注册到多路复用器上面，所以所有的连接只需要一个线程就可以搞定，当这个线程中的多路复用器进行轮询的时候，发现连接上有请求的话，才开启一个线程进行处理，也就是一个请求一个线程模式。
* 在NIO的处理方式中，当一个请求来的话，开启线程进行处理，可能会等待后端应用的资源(JDBC连接等)，其实这个线程就被阻塞了，当并发上来的话，还是会有BIO一样的问题。
* HTTP/1.1出现后，有了HTTP长连接，这样除了超时和指明特定关闭的HTTP header外，这个链接是一直打开的状态的，这样在NIO处理中可以进一步的进化，在后端资源中可以实现资源池或者队列，
当请求来的话，开启的线程把请求和请求数据传送给后端资源池或者队列里面就返回，并且在全局的地方保持住这个现场(哪个连接的哪个请求等)，这样前面的线程还是可以去接受其他的请求，而后端的应用的处理只需要执行队列里面的就可以了，这样请求处理和后端应用是异步的。
当后端处理完，到全局地方得到现场，产生响应，这个就实现了异步处理。

  与NIO不同，当进行读写操作时，只须直接调用API的read或write方法即可。这两种方法均为异步的，对于读操作而言，当有流可读取时，操作系统会将可读的流传入read方法的缓冲区，并通知应用程序；对于写操作而言，当操作系统将write方法传递的流写入完毕时，操作系统主动通知应用程序。即可以理解为，read/write方法都是异步的，完成后会主动调用回调函数。
在JDK1.7中，这部分内容被称作NIO.2。


延申：

Reactor设计模式（反应器）

主线程（IO处理单元）只负责监听socket文件描述符上是否有事件发生，有的话就立即将该事件通知工作线程(逻辑处理单元)。除此之外，主线程不做任何其他实质性的工作，读写数据、接受新的连接请求、处理客户端请求均在工作线程中完成。 

![](https://github.com/jxnu-liguobin/Java-Learning-Summary/blob/master/src/cn/edu/jxnu/practice/picture/Reactor%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F.png)

Proactor设计模式（前摄器）

将所有IO事件操作都交由主线程和内核处理，工作线程只负责业务逻辑

![](https://github.com/jxnu-liguobin/Java-Learning-Summary/blob/master/src/cn/edu/jxnu/practice/picture/Proactor%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F.jpg)

  Proactor模式的读写socket操作是交由内核处理，即异步IO模式，但是编程使用的IO复用技术是同步IO，我们可以利用同步IO方式模拟出Proactor模式，其原理为：主线程执行数据的读写操作(和监听listen_socket)，读写完成后主线程向工作通知这一“完成事件”，那么从工作线程的角度看，和异步IO实现的Proactor模式的工作线程一样，它们直接获得了数据的读写结果，接下来要做的只是对读写结果进行逻辑处理。


参考[CSDN](https://blog.csdn.net/ty497122758/article/details/78979302)