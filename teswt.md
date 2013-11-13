MINICORE

## 1.基本概念##
#### 1.1.执行流、服务体、控制流
  控制流：控制流是程序执行中所有可能的事件顺序的一个抽象表示。   
  执行流：执行流代表单个处理器物理核心执行的指令序列。
  服务体：拥有独立地址空间、独立代码和数据的能够完成一定功能或提供服务的静态数据结构。

#### 1.2.服务、事务端口、服务端口、小端口
  服务：提供给非本体完成某些功能的过程。
  事务端口：代表完成某项任务功能的静态数据结构。
  服务端口：代表提供某种服务的静态数据结构。
  小端口：端口完成任务或者提供功能时候产生的运行实例。
## 2.内存管理
  地址空间：多地址空间，每个服务体有一个地址空间、相互之间隔离，但是各个服务体地址空间都和核心服务体地址空间一一映射。
  栈：用户数据都在栈上完成分配，内核数据部分在栈上分配。
  对象池：内核对象则直接通过对象池管理器进行分配。
## 3.任务管理
### 3.1.任务类型
  IO密集型任务、计算密集型任务、访存密集型任务。
### 3.2.任务定义
  广义的讲，进程模型中，任务等同于进程。为此，我们将任务定义为控制流的一部分，以控制流进入事务端口代表任务启动，以控制流退出该事务端口代表任务结束。
### 3.3.任务操作
  任务的创建
### 3.3.周期、非周期、偶发
  任务包括周期性释放的周期任务、在非周期可预料时间释放的非周期任务以及在不可预料的时间释放的偶发任务。
    MINICORE中，任务用事务端口代表，在实现的时候有两种方案。
   <p>
   <font color = Crimson>
    注意：为什么我们没有抽象TCB的概念？
    <br>传统的进程模型中，所有和任务相关的资源都在进程中得以体现。而在我们的系统中，静态的数据由服务体和端口来体现，动态的数据由小端口体现，传统的TCB的概念已经被分散在了不同的数据结构中。
   </font>
   </p>
    当然，为了方便管理，我们仍然需要提出TCB的概念。我们考虑了两种方案。
    1. 由事务端口来代表TCB的数据结构， 
    <p><font>
           
```c
typedef struct MINIPORT{
    UINT32 MiniportId;
    UINT32 Priority;
    BOOLEAN IsPeriod;
    UINT32 Period;
    UINT32 DeadLine;
    CONTEXT Context;        
} STRUCT_MINIPORT;

typedef struct TPORT{
    UINT32 TPortId;
    MINIPORT Miniport;
    UINT32 Entry;
    DLINK TPortLink;
    
    UINT32 StaticPriority;
}

typedef struct PORT{
    UINT32 PortId;
    MPORTLIST FreeMiniportList;
    MPORTLIST ActiveMiniportList;
    UINT32 Entry;
    DLINK PortLink;
} STRUCT_PORT;

typedef struct SERVANT{
    UINT32 ServandId;
    UINT32 StartCode,EndCode,StartData,EndData;
    STRUCT_PORT TransactionPort;
    PORTLIST ServicePortList;
    DLINK ServantLink;
} STRUCT_SERVANT;
```
  </p>

      
    
    
### 3.4.静态、动态调度
  首先，作为实时系统，我们采用的调度策略为实时调度策略。
  而考虑到应用场景所要求的实时性，我们选择硬实时系统。
  为了提高系统的灵活性，我们提供给用户可配置的混合调度，通过配置，用户可以将系统调度设置为静态调度、动态调度或者混合调度。
  调度策略则包括静态调度策略（RM，在静态调度算法为最优），动态调度策略（EDF，CPU利用率高）。同样可以根据需要对调度策略进行配置。
  考虑到抢占性对实时系统的重要性，我们选择可抢占调度。
  上述的调度策略都是基于优先级和时间片轮转的调度策略，在系统中，我们需要实现最基本的基于优先级和时间片轮转的调度策略。

       
  * 任务模型
  * 任务调度
  * 编程模型
  * 通信管理
  * 中断管理
  * 时钟管理
  * 定时器管理
  * 同步
  * 实时性
  * 容错性
<p><font color = green>addidas</p>
