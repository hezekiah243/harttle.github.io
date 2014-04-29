---
layout: article
title:  Modern Operating Systems
categories: 读书
tags: 读书笔记 操作系统
excerpt: '"Modern Operating Systems", Andrew S. Tanenbaum, 机械工业出版社'
---

# Introduction

## What is OS

1. **extendes machines** : providing programmers(and programs) a clean abstract set of resources instead of the messy hardware
2. **resource manager** : managing hardware resources


## History of OS

The first digital computer was designed by Charles Babbage. Ada Lovelace was the first programmer(hired by Babbage).

### 1st Generation Vacuum Tubes

* The first functioning digital computer, by John Atanasoff and Clifford Berry, Iowa State University.
* Z3, Konrad Zuse, Berlin.
* Colossus, a group at Bletchey Park, England
* Mark I, Howard Aiken, Harvard
* ENIAC, William Mauchley and J. Presper Eckert, University of Pennsylvania

<!--more-->

### 2nd Generation Transistors and Batch System

**batch system** 

1. Programmers bring cards to IBM 1401
2. 1401 read batch of jobs onto tape
3. Operator carries input tape to IBM 7094
4. 7094 does computing
5. Operator carries output tape to 1401
6. 1401 prints output

### 3rd Generation ICs and Multiprogramming

**IBM System/360** 

A series of software-compatible machines differed only in price and performance.

360 was the first major computer line to use **ICs(Integrated Circuits)**.

**OS/360** had to work on all models, the result was an enormous and complex OS, each release fixed some bugs and introduced new ones.

**multiprogramming** 

Partition memory into several pieces, with a different job in each partition. While one job was waiting for I/O, another job could be using the CPU.

**spooling** 

Simultaneous Peripheral Operation On Line, OS read jobs from cards onto the disk as soon as they were brought to the computer room, whenever a running job finished, the operating system could load a new job from the disk and run it.

**timesharing**

Proveding quick response time. The 1st general-purpose timesharing system, **CTSS(Compatible Time Sharing System) was developed at MIT.

**MULTICS(MULTiplexed Information and Computing Service)**

Developed by MIT, Bell Labs, and General Electric, MULTICS supports handreds of uses on a machine only slightly powerful than an Intel 386-based PC.

**minicomputer**

DEC PDP-1(1961) and other PDPs(all incompatible)

**UNIX**

Developed by Ken Thompson(based on PDP-7  minicomputer). There are 2 major versions: 

**System V**(from AT&T) and **BSD**(Berkeley Software Distribution)

1987, **MINIX** was released for educational purposes.

### 4th Generation Personal Computers

With the development of **LSI**(Large Scale Integration) circuits, **microcomputers** appears.

 Kildall developed **CP/M**(Control Program for Microcomputers, disk-based OS), which later supports 8080, Zilog Z80, and other CPU chips.

Bill Gates offered **DOS**(Disk Operating System, which renamed to MS-DOS later) to IBM.

Engelbart invented the **GUI**(Graphical User Interface), which was adopted by Xerox PARC.

**Apple**

Steve Jobs visited PARC and embarked on building an Apple with a GUI.

**Windows**

Microsoft released Windows 95, Windows 98(with 16-bit Intel CPU), Windows NT(New Technology, 32-bit), Windows Me(Millennium edition), Windows 2000(1999,renamed from Windows NT5), and Windows XP(2001).

**UNIX**

FreeBSD, originating from BSD project at Berkeley. X Window System(X11), MIT.

## OS ZOO

**Mainframe OS**

Oriented toward process many jobs at once, most of which need prodigious amounts of I/O.

**Server OS**

Serve multiple users at once over a network and allow the users to share hardware and software resources.

**Mutiprocessor OS**

Special features for communication, connectivity, and consistency.

**Personal Computer OS**

Provide good surport to a single user.

**Handheld Computer OS**

**PDA**(Personal Digital Assistant) and mobiles.

**Embedded OS**

Donot accept user-installed software.

**Sensor Node OS**

Tiny computers that communicate with each other and with a base station using wireless communication.

**Real-Time OS**

**hard real-time** the action absolutely must occur at a certain moment.

**soft real-time** missing an occasional deadline is acceptable and does not cause any permanent damage.

**Smart Card OS**

Some are Java-oriented. The ROM holds an interpreter for the JVM. 
Resource management and protection.

## OS Structure

### Monolithic Systems

Basic sturctures

1. A main program that invokes requested service procedure.
2. A set of service procedures that carry out the system calls.
3. A set of utility procedures that help the service procedures.

### Layered Systems

**THE** system built by E. W. Dijkstra.

|layer|function|
|:---:|:---|
|5|The operator|
|4|User programs|
|3|I/O management|
|2|Operator-process communication|
|1|Memory and drum management|
|0|Processor allocation and multiprogramming|

**MULTICS** was described as having a series of concentric rings, with the inner ones being more privileged than the outer ones.

> The advantage is that it can be easily extended to structure user subsystems.

### Microkernels

Achieve high reliability by spilitting the OS into small, well-defined modules, only one of which(the microkernel) runs in kernel mode.

An idea related to having a minimal kernal is to put the **mechanism** for doing something in the kernel but not **policy**.

> A few of better-known microkernels: Integrity, K42, Symbian, and MINIX 3.

### Client-Server Model

A slight variantion of the microkernel idea is to distinguish 2 classes of processes, the **servers**(providing services), and the **clients**(use these services).

### Virtual Machines

#### Type 1 hypervisor

Also known as **virtual machine monitor**, runs upon th e hardware.

**CP/CMS** , later renamed VM/370 is a timesharing system provedes 

1. multiprogramming 
2. extended machine with a more convenient interface than the bare hardware

**CMS** (Conversational Monitor System), a single-user, interactive processing OS.

#### Type 2 hypervisor

Runs upon the Host OS, like other applications.

### Exokernels

Rather than cloning the actual machine, another strategy is pratitioning it, giving each user a subset of the resources.

At the bottom layer, running in kernel mode, is a program called the **exokernal**.

# Processes and Threads

## Processes

**pseudoparallelism**

The illusion of parallelism while CPU is switching from process to process quickly.

### The Process Model

**process** is an instance of an executing program, is the activity of a program.

> If a program is running twice, it counts as two processes.

### Process Creation

When processes are created

1. System init
2. Process creation system call by a running process
3. A user request to create a new process.
4. Init of a batch job.

**fork**

In Unix, this call creates an exact clone of the calling process. `execve` is another syscall used to change its memory image and run a new program.

> This separation allows the child to manipulate its file descriptors after `fork` but before the `execve` in order to accomplish redirections.

**CreateProcess**

In Windows, this call handles both process creation and loading with 10 parameters.

> It's possible for a newly created process to share resources such as open files.

### Process Termination

When processes terminate

1. Normal exit (voluntary)
2. Error exit (voluntary)
3. Fatal error (involuntary)
4. Killed by another process (involuntary)

Voluntary termination: `exit` in UNIX and `ExitProcess` in Windows. Kill someother process: `kill` in UNIX and `TerminateProcess` in Windows.

### Process Hierarchies

In UNIX, a process and all its children and further descendants form a process group. User signals are sends to all members in the group.

> `init` is the first process created in boottime. Thus all processes in the whole system belong to a single tree with `init` as the root.

Windows has no concept of process hierarchy, despite that a handle(a special token used to control the child) is returnd when creating a new process.

### Process States

1. Running (actually using the CPU)
2. Ready (runnable; temporariy stopped to let another process run)
3. Blocked (unable to run util some external event happens)

### Implementation of Processes

The OS maintains a **process table** , with one entry ( **process control block** ) per process. Each entry contains program counter, stack pointer, memory allocation, status of open files, scheduling information registers, and everything need to save when swapped out.

Interrupt routine

1. Hardware stacks program counter, etc
2. Hardware loads new program counter from interrupt vector
3. Assembly language procedure saves registers.
4. Assembly language procudure sets up new stack.
5. C interrupt service runs (typically reads and buffers input).
6. Scheduler decides which process is to run next.
7. C procedure returns to the assembly code.
8. Assembly language procedure starts up new current processes.

### Modeling Multiprogramming

Suppose p is the fraction of I/O time for a process. n is current count of processes. Then

$CPU~utilization = 1 - p^n$

where n is called the **degree of multiprogramming** .

## Threads

### Thread Usage

1. Multiple activities going on at once with the ability to share an address space
2. Lighter weight than processes, easy to create and destroy.
3. Overlap activities with substantial I/O to speeding up the application.
4. Useful on systems with multiple CPUs.

Three ways to construct a server

1. Thread. Parallelism, blocking system calls.
2. Single-threaded process. No parallelism, blocking system calls.
3. Finite-state machine. Parallelism, nonblocking system calles, use interrupts to simulate thread.

### Classical Thread Model

Processes are used to group resources together; threads are the entities scheduled for execution on the CPU. There are no protection betwwen threads because (1) it's impossible, and (2) it should not be nessessary.

While threads share one memory space, it takes fewer space to maintain a thread, including Program Counter, Registers, Stack and State.

### POSIX Threads

`Pthread_create`: create a new thread.
`Pthread_exit`: terminate the calling thread.
`Pthread_join`: wait for a specific thread to exit.
`Pthread_yield`: release the CPU.
`Pthread_attr_init`: create and initialize a thread's attribute structure.
`Pthread_attr_destroy`: remove a thread's attribute structure.

### Implementing Threads in User Space

Advantages

1. useful on an OS that doesn't support threads
2. switching is faster than trapping to the kernal
3. allow process have its own scheduling algorithm

Problems

1. Implementation of blocking sys-calls. 

    These calles intended to block the process (all threads in it) since the kernal know nothing about threads. This problem could be solved by adding **wrappers** to all blocking sys-calls.
2. Page faults.

    The same as above.
3. A running thread never voluntarily gives up the CPU since no clock interrupts in user space.
4. Substantial sys-calles are needed generally in threads. It's hardly any more work for the kernel to switch threads.

### Implementing Threads in the Kernel

> Due to the relatively greater cost of creating and destroying threads in the kernel, some systems take an environmentally correct approach and recycle their threads.

Problems

1. When a multithreades process forks
2. Signals sent to a multithread process

### Hybrid Implementations

Programmers can determine how many kernel threads to use and how many user-level threads to multiplex on each one.

### Scheduler Activations

The kernel notifies the process' run-time system to switch thread, thus avoiding unnecessary transitions between user and kernel space.

> This implementation violates the structure inherent in any layered system.

### Pop-Up Threads

On arrival of a message, the system creates a new thread to handle it. Since a pop-up thread has no history, it's quicker to create than swap.

### Making Single-Threaded Code Multithread

Problems should be solved

1. global variables

    Private global variables. A new library to create, set, read these variables is needed.
2. many library procedures are not reentrant

    A jacket for each of these procedures is needed.
3. signals
4. stack management

    overflows could not be awared.

## Interprocess Communication

**race conditions**

Two or more processes are reading or writing some shared data and the final result depends on who runs precisely when.

### Critical Regions

**mutual exclusion**

Prohibit more than one process from reading and writing the shared data at the same time.

**critical region**

Also called **critical section** , the part of the program where the shared memory is accessed.

Conditions to a good solution

1. No two processes may be simultaneously inside their critical regions
2. No assumptions may be made about speeds or the number of CPUs
3. No process running outside its critical region may block other processes
4. No process should have to wait forever to enter its critical region

### Mutual Exclusion with Busy Waiting

#### Disable Interrupts

Problems

1. It's unwise to give user processes the power to turn off interrupts.
2. It's convenient for the kernel itself to disable interrupts for a few instructions, when race conditions could occur.

#### Lock Variables

This would not take place without atom operations.

#### Strict Alternation

```cpp
//Process 1
while(TRUE){
    while (turn != 0);
    critical_region();
    turn = 1;
    noncritical_region();
}

//Process 2
while(TRUE){
    while(turn != 1);
    critical_region();
    turn = 0;
    noncritical_regino();
}
```

Problem: neither of them could run twice in a row, which violates condition 3.

> Continuously testing a variable until some value apperas is called **busy waiting** , a lock that uses busy waiting is called a **spin lock** .

#### Peterson's Solution

Busy waiting lock variables.

```cpp
#define FALSE 0
#define TRUE 1
#define N 2

int turn;
int interested[N];

void enter_region(int process)
{
    int other = 1 - process;
    interested[process] = TRUE;
    turn = process;
    while (turn == process && interested[other] == TRUE);
}

void leave_region(int process)
{
    interested[process] = FALSE;
}
```

#### TSL Instruction

```
TSL RX, LOCK 
``` 
Test and lock lock instruction reads the content of the memory word `lock` into register `RX` and then stores a nonzero value at the memory address `lock`.

> It's guaranteed by the hardware that the read and set operations are indivisible.

```
enter_region:
    TSL REGISTER, LOCK
    JNE REGISTER, #0, enter_region
    RET     'return

leave_region:
    MOVE LOCK, #0
    RET
```

An alternative instruction to `TSL` is `XCHG`. The implementations are similiar.

### Sleep and Wakeup

Problems in busy waiting:

1. wasting CPU time
2. process with higer priority will keep busy waiting while lower priority process never run

#### The Product-Consumer Problem

```python
Producer loop:
    if count == N:
        sleep
    else:
        produce one
    if count == 1:
        wakeup consumer

Consumer loop:
    if count == 0:
        sleep
    else:
        consume one
    if count == N-1
        wakeup producer
```

Since `count` is unconstrained, race condition could occur. When consumer is about to sleep, wakeup signal from producer is lost, causing both of them sleeping.

#### Semaphores

Semaphores are used to buffer signals, keep them from losing. Value 0 indicating that no wakeups were saved; positive value if some wakeups were pending.

`down`(Proberen, try in Dutch) operation: checking the value and cosume one or sleep to wait one.
`up`(Verhogen, raise in Dutch) operation: produce one, if someone's waiting, wake him.

### Mutexes

**mutex** 

A kind of semaphore, a variable that can be in 1 of 2 states: unlocked or locked, used when the semaphore's ability to count is not needed.

### Monitors

When several mutexes are refered to, deadlock could occur by a subtle error. **Monitors** are provided by some programming languages to manage a group of mutual exclusive threads. 

> Only one of the threads in this group would run in a certain time. It's up to the compiler to arrange mutexes to accomplish the monitor.

### Message Passing

**message passing** is used for information exchange between machines. This method of interprocess communication uses 2 primitives, `send` and `receive`.

Approaches:

1. **mailbox** 

    A mailbox is a place to buffer a certian number of messages.
2. **rendezvous**

    No buffering, either of each primitive is blocked until the other occurs.

### Barriers

With multiple processes, a **barrier** can be placed at the end of each phase. When a process reaches the barrier, it's blocked until all processes have reached the barrier.

## Scheduling

### Introduction to Scheduling

#### Personal Computer

Scheduling is simpler in personal computers because (1) there is only one active process, (2) CPU is more faster than I/O.

#### Process Behavior

Compute-bound: long CPU bursts and thus infrequent I/O waits.
I/O-bound: short CPU bursts and thus frequent I/O waits.

#### When to Schedule

1. A new process is created
2. A process exits
3. A process blocks on I/O
4. I/O interrupt occurs

Types of schedule algorithm

1. **nonpreemptive** scheduling algorithm: picks a process to run until it blocks, exit, or voluntarily release the CPU.
2. **preemptive** scheduling algorithm: picks a process and lets it run for a maximum of some fixed time.

#### Goals of Scheduling Algorithms

All systems

* Fainess - giving each process a fair share of the CPU
* Policy enforcement - seeing that stated policy is carried out
* Balance - keeping all parts of the system busy

Batch systems

* Throughput - maximize jobs per hour
* Turnaround tiem - minimize time between submission and termination
* CPU utilization - keep the CPU busy all the time

Interactive systems

* Response time - respond to requests quickly
* Propertionality - meets user's expectations

Real-time systems

* Meeting deadlines - avoid losing data
* Predictability - avoid quality degradation in multimedia systems

### Scheduling in Batch Systems

#### First-Come First-Served

#### Shortest Job First

Reduce the mean turnaround time.

#### Shortest Remaining Time Next

When new process enterd, its executime is compared with the remaining time of current process.

### Scheduling in Interactive Systems

#### Round-Robin Scheduling

Each process is assigned a time interval, called its **quantum** , during which it is allowed to run. The CPU switches when the process blocks, of course.

Setting the quantum too short causes too many process switches and lowers the CPU efficiency, but setting it too long may cause poor response to short interactive requests. A quantum arount 20-50 msec is often a resnable compromise.

#### Priority Scheduling

Each process is assigned a priority, and the runable process with the highest priority is allowed to run. The priority of the running process decreases at each clock tick.

> A simple algorithm for giving a good service to I/O bound processes is to set the priority to 1/f, where f is the fraction of the last quantum that a process used.

#### Multiple Queues

Set up priority classes, a group of processes sorted by priority in each class. 

Whenever a process used up all the quanta allocted to it, it was moved down one class, saving the CPU for short, interactive processes.

Whenever a carriage return was typed at a terminal, the process belonging to that terminal was moved to the highest priority class. 

> This prevents a process that needed to run for a long time when it first started but became interactive later, from being punished forever.

#### Shorted Process Next

To a certain extent, it would be nice if this algorithm used in batch systems could be used for interactive processes.

**aging**

Estimating running time as $t = aT_0 + (1-a)T_1$

#### Guaranteed Scheduling

Make real promises to the users about performance ahd then live up to those promises.

1. Compute the ratio of actual CPU time consumed to CPU time entitled.
2. Run the process with the lowest ratio until its ratio is no longer the lowest.

#### Lottery Scheduling 

Give processes lottery tickets for various system resources. Whenever a scheduling decision has to be made, a lottery ticket is chosen at random, and the process holding that ticket gets the resource.

Advantages

1. lottery scheduling is highly responsive, the process have more lottery tickets is more likely to get the turn.
2. processes can exchange lottery tickets, the server and the client for example.

#### Fair-Share Scheduling

Each user is allocated some fraction of the CPU and the scheduler picks processes in such a way to enforce it.

For example, user 1 created process A B C D, while user 2 only created process E, the scheduling sequence should be:

A E B E C E ...

If user 1 is entitled to twice as much CPU time as user 2, the sequence should be:

A B E C D E ...

### Scheduling in Real-Time Systems

The events that a real-time system may have to respond to can be categorized as **periodic** (occurring at regular intevals) or **aperiodic** (occurring unpredictably).

Depending on how much time each event requires for processing, it may not even be possible to handle them all. A real-time system that meets this requirement is said to be **schedulable** .

### Policy versus Mechanism

Separate the scheduling mechanism from the scheduling policy to alow more flexible scheduling. What this means is that the scheduling algorithm is parameterized in some way.

### Thread Scheduling

![thread-scheduling](/assets/img/blog/thread-scheduling.png)

* Kernel-level thread doesn't block the entire process when blocking sys-calls are made, unlike user-level thread.
* Since the kernel knows that switching from a thread in process A to a thread in process B is more expensive than running a second thread in process A, it can take this into account when making a decision.
* User-level threads can employ an application-specific thread scheduler.

# Memory Management

## No Memory Abstraction

Running multiple programs by **static relocation**: Modify the second program on the fly as it loaded it into memory.

> The lack of memory abstraction is still common in embedded and smart card systems.

## A Memory Abstraction: Address Spaces

**Address space** is the set of addresses that a process can use to address memory.

**Dynamic relocation** uses base and limit registers to map each process' address space onto a different part of physical memory in a simple way.

> The disadvantage of relocation using base and limit registers is the need to perform an addition and comparison on every memory reference.

**Swapping** is used to deal with memory overload, bringing in each process in its entirety, running for a while, then putting it back on the disk.

> When swapping creates multiple holes in memory, it's possible to combine them all into one big one, which is called **memory compaction**.
> Free memory can be recorded as bitmaps or linked lists.

## Virtual Memory

Processes use program generated addresses, called **virtual address**. They go to an **MMU(Memory Management Unit)** that maps the virtual addresses onto the physical addresses when a memory access occurs.

> **virtual memory** allows programs to run even when they are only partially in main memory.

### Page Tables

The virtual address space is divided into fixed-size units called **pages**. The corresponding units in the physical memory are called **page frames**.

The virtual address is split into a virtual page number and an offset. The virtual page number is used as and index into the page table to find the entry for that page. Each **page table entry(PTE)** consists a Caching disabled bit, Referenced bit, Modified bit, Present/absent bit and page frame number.

### TLB

**TLB(Translation Lookaside Buffers)** is used to speed up paging. It's usually inside the MMU and consists of a small number of entries. Each entry contains information about one page, including the virtual page number, a bit that is set when the page is modified, the protection code, and the physical page frame in which the page is locted.

### Page Tables for Large Memories

**multilevel page table** avoids keeping all the page tables in memory all the time.

**Inverted page table** is a solution for 64-bit computers. There is one entry per page frame in real memory, rather than one entry per page of virtual address space.

### Page Replacement Algorithms

#### The optimal page replacement algorithm

Each page can be labeled with the number of instructions that will be executed before that page is first referenced. And the page with the highest label should be removed.

> The only problem with this algorithm is that it's unrealizable.

### The Not Recently Used Page Replacement Algorithm

### The First-in, First-out Page Replacement Algorithm

### The Second-Chance Page Repcacement Algoritm

### The Clock Page Replacement Algorithm

### The Least Recently Used Page Replacement Algorithm

### The Working Set Page Replacement Algorithm

Keep track of each process' working set and make sure that it is in memory before letting the process run, which is called **prepaging**.

# File System

## Files

### File structure

* Byte sequence
* Record sequence
* Tree

### File Types

* Regular files
* Directories
* Character special files
* Block special files

### File operations

* Create
* Delete
* Open
* Close
* Read
* Write
* Append
* Seek
* Get attributes
* Set attributes
* Rename

## Directories

Forms of directory system:

* Single-Level Directory Systems
* Hierarchical Directory Systems

###Path names

**Absolute path name**: Consists the path from the root directory to the file.
**Relative path name**: This is used in conjunction with the concept of the *working deirectory*(*current directory*)

### Directory operations

* Create
* Delete
* Opendir
* Closedir
* Readdir
* Rename
* Link
* Unlink

## File system implementation

### File System Layout

**MBR(Master Boot Record)** is the first sector(sector 0) of the disk, which contains the partition table(one is marked as active). 

**Superblock** is contained by every partition, which lays after the boot block and contains all the key parameters about the filesystem.

> MBR is loaded and executed by BIOS. The MBR program locate the active prtition, read in its first block(**boot block**), and execute it. The the program in the boot block load the OS contained in that partition.

![file system layout](/assets/img/blog/4-9.png)

### Implementing Files

* Contiguous Allocation
* Linked List Allocation
* Linked List Allocation Using a Table(File Allocation Table) in Memory
* I-nodes

### Implementing Directories

Longer, variable-length file names supporting. There are 3 implementations below:

* Set a limit on file name length.
* Each entry contains a fixed portion, followed by the actual file name.
* Make the directory entries all fixed length and keep the file names together in a heap at the end of the directory.

### Shared Files

2 solutions:

1. Disk blocks are not listed in directories, but in a little data structure associated with the file itself.
2. **Symbolic linking** The new link file just contains the path of the file to which it is linked.

### Log-Structured File Systems

All pending writes are bufferd in memory, and collected into a single segment and written to the disk as a single contiguous segment at the end of the log.

> An i-node map is maintained to make it possible to find i-nodes.
> LFS has a cleaner thread that spends its time scanning the log circularly to compact it. (Removing overwitten blocks)

### Journaling File System

Keeps a log of what the file system is going to do before it does it, so that if the system crashes before it can do its planned work, upon rebooting the system can look in the log to see what was going on at hte time of the crash and finish the job.

> Only after the log entry has been written, do the various operations begin. After the operations complete successfully, the log entry is erased. 
> The logged operations must be **idempotent**.

### Virtual File System

**VFS** tries to integrate multiple file systems into an orderly structure.

> In fact, the origina motivation for Sun to build the VFS was to support remote file systems using the **NFS(Network File System)** protocol.

## File System Management and Optimization

### Disk Space Management

#### Block Size

If the allocatoin unit is too large, we waste space; if it's too small, we waste time.

> The disk operation time is the sum of the seek, rotational delay, and transfer time. While the first 2 of them dominated the access time.

#### Keeping Track of Free Blocks

1. Linked list.
2. bitmap.
3. Keep track of runs of blocks(rather than single blocks).

### File System Backups

* Incremental dump
* physical dump
* logical dump

### File System Consistency

* Block consistency. Count how many times each block is present in a file; how often each block is present in the free list.
* File consistency. Count for that file's usage count(hard links, directories).

### File System Performance

* Caching. Hash the device and disk address and look up the result in a hash table.
* Block Read Ahead. Only works for files that are being read sequentially.
* Reducing Disk Arm Motion. I-node allocation.


# Input/Output

## Principles of I/O Hardware

### Devices & Controllers

* **block devices** stores information in fixed-size blocks, each one with its own address.
* **character devices** delivers or accepts a stream of characters, without regard to any block structure.
* **device controller/adapter** takes the form of a chip on the parentboard or a printed circuit card that can be inserted into a (PCI) expansion slot.

### Memory-Mapped I/O

Each control register is assigned a unique memory address to which no memory is assigned.

Pros:

* if special I/O instructions are needed to read/write the device control registers, access to them requires nothing more than standard C code, otherwise needs the use of addembly code.
* with memory-mapped I/O, no special protection mechanism is needed to keep user processes from performing I/O.
* With memory-mapped I/O, every instruction that can reference memory can also reference control registers.

Cons:

* most computers nowadays have some form of caching of memory words. Caching a device control register would be disastrous.
* If there is only one address space, then all memory modules and all I/O devices must examine all memory references to see which ones to respond to.

### Direct Memory Access(DMA)

**Direct memory access (DMA)** is a feature of modern computers that allows certain hardware subsystems within the computer to access system memory independently of the CPU.

Bus mode thus DMA controller mode:

* **cycle stealing** the device controller sneaks in and steals an occasional bus cycle from the CPU once in a while(when CPU doesn't want the bus).
* **burst mode** the DMA controller tells the device to acquire the bus, issue a series of transfers, then release the bus.

### Interrupts Revisited

* **interrupt vector** is used to save program counters for a table of the corresponding interrupt service procedure.
* Most CPUs save the context on the stack when interrupt happens.

**Precise interrupt**

1. The PC is saved in a known place.
2. All instructions before the one pointed to by the PC have fully executed.
3. No instruction beyond the one pointed to by the PC has been executed.
4. The execution state of the instruction pointed to by the PC is known.

An interrupt doesn't meet these requirements is called an **imprecise interrupt**.

## Principles of I/O Software

### Goals of I/O Software

* **Device independece** programs can access any I/O device without having to specify the device in advance.
* **uniform naming** thus a path name in UNIX systems.
* **error handling** try to correct the error or told upper layers.
* **synchronous** or **asynchronous**
* **buffering**

## I/O Software Layers

1. User-level I/O software
2. Device-independent operating system software
3. Device drivers
4. Interrupt handlers
5. Hardware

### Device-Independent I/O Softwares

####Uniform Interfacing for Device Drivers

* The interface between the device drivers and the rest of the operating system.
* How I/O devices are named.

> in Unix a device name such as `/dev/disk0/`, uniquely specifies the i-node for a special file. This i-node contains **major device number** to locate the appropriate driver, and the **minor device number** which is passed as a parameter to the driver and specifies the read/write unit.

####Buffering

**double buffering** is used to store another buffer when the first buffer is being brought out and characters are keeping arriving.
**circular buffer** is another widely used form of buffering.

# Deadlocks

## Resources

A **preemptable resource** is one that can be taken away from the process owning it with no ill effects.

A **nonpreemptable resource** is one that cannot be taken away from its current owner without causing the computation to fail.

Resource Acquisition

1. Request the resource.
2. Use the resource.
3. Release the resource.

A possible implementation:

1. `down` on the semaphore to aquire the resource.
2. using the resource.
3. `up` on the semaphore to release the resource.

## Introduction to Deadlocks

A set of processes is **deadlocked** if each process in the set is waiting for an event that only another process in the set can cause.

Conditions for Resource Deadlocks:

1. Mutual exclusion condition.
2. Hold and wait condition.
3. No preemption condition.
4. Circular wait condition.

Strategies used for dealing with deadlocks:

1. Just ignore the problem.
2. Detection and recovery.
3. Dynamic avoidance by careful resource allocation.
4. Prevention, by structurally negating one of the four required conditions.

## The Ostrich Algorithm

Just ignore the problem when the deadlock isn't that often, that serious.

> Few current systems will detect the deadlock between CD-ROM and printer.

## Deadlock Detection and Recovery

### Deadlock Detection with One Resource of Each Type

For such a system, we can construct a resource graph of the resources and processes. If this graph contains cycles, a deadlock exists.

### Deadlock Detection with Multiple Resources of Each Type

E is the **existing resource vector**, which gives the total nmber of instances of each resource in existence.

A is the **available resource vector**, which gives the number of instances of resource that are currently available.

C is the **current allocation matrix**, the i-th row of C tells how many instances of each resource class $P_i$ currently holds.

R is the **request matrix**, holds the number of instances of resource that processes want.

The following algorithm will mark all processes that are not deadlocked.

1. Look for an unmarked process, $P_i$, for which the i-th wor of R is less than or equal than A.
2. If such a process is found, add the i-th row of C to A, mark the process, and go back to step 1.
3. If no such process exists, the algorithm terminates.

### Recovery from Deadlock

* Recovery from Preemption
* Recovery through Rollback, which requires processes periodically **checkpointed**(memory image and resource state).
* Recovery through Killing Processes.

## Deadlock Avoidance

### Resource Trajectories

![trajectories](/assets/img/blog/6-8.gif)

### Safe and Unsafe States

A state is said to be **safe** if there is some scheduling order in which every process can run to completion even if all of them suddenly request their maximum number of resources immediately.

> It worth nothing that an unsafe state is not a deadlocked state. The system can run for a while and one process can even complete. The difference between a safe state and an unsafe state is that from a safe state the system can **guarantee** that all processes will finish; from an unsafe state, no such guarantee can be given.

### The Banker's Algorithm for a Single Resource

This Algorithm considers each request as it occurs, and sees if granting it leads to a safe state. If it does, the request is granted; otherwise, it is postponed until later.

### The Banker's Algorithm for Multiple Resources

1. Look for a row, R, whose unmet resource needs are all smaller than or equal to A. If no such row exists, the system will eventually deadlock since no process can run to completion.
2. Assume the process of the row chosen requests all the resources it needs and finishes. Mark that process as terminated and add all its resources to the A vector.
3. Repeat steps 1 and 2 until either all processes are marked terminated or no process is left whose resource needs can be met.

## Deadlock Prevention

Condition | Approach
--------- | --------
Mutual exclusion | Spool everything
Hold and wait | Request all resources initially
No preemption | Take resources away
Circular wait | Order resources numerically

## Other Issues

**two-phase locking** 

1. In the first phase, the process tries to lock all the records it needs, one at a time. 
2. If it succeds, it begins the second phase, performing its updates and releasing the locks.
3. If during the first phase, some record is already locked, the process just release all its locks and starts the first phase all over.

**conmunication deadlocks**, unlike resource deadlocks, these are caused by communication errors.

**livelock**, polling(busy waiting) processes uses up its CPU quantum without making progress but also without blocking.

**Starvation**, some policy is needed to make a decision about who gets which resource when. This policy may lead to some processes never getting service even though they are not deadlocked.

> Starvation can be avoided by using a first-come, first-served, resource allocation policy.

# Case Study: Linux

## History of Unix and Linux

### MULTICS & UNICS

* Researchers at M.I.T. joined forces with Bell Labs and Generic Electric and began designing a second-generation system, **MULTICS(Multiplexed Information and Computing Service)**
* One of the Bell Labs researchers, Ken Thompson wrote a stripped-down MULTICS on a PDP-7 minicomputer. The system is called **UNICS(UNiplexed Information and Computing Service)** jokingly by Brian Kernighan, Bell Labs.
* Thompson tied to rewite UNIX with B language of his own design and failed. Ritchie then designed a successor to B, called C. Working together, they rewrote UNIX in C.
* Steve Johnson of Bell Labs designed and implemented the **portable C compiler**, which could be retargeted to produce code for any resonable machine with a only a moderate amount of effort.

### Standard UNIX

* AT&T released its first commercial UNIX: System III, then System V.
* Aided by U.S. Dept. of Defense, Berkeley released an improved PDP-11 called **1BSD(First Berkeley Software Distribution)**, which supported virtual memory, TCP/IP, vi, csh, etc.
* IEEE came up with **POSIX(Portable Operating System of UNIXish)**, witch reconcile the two flavors of UNIX.

### MINIX & Linux

* Tanenbaum wrote a Unix-like MINIX based on a microkernel design.
* In 1991, Linus Torvalds wrote another UNIX, named **Linux**, according to MINIX.
* Linux comes with GPL Lisence devised by Richard Stallman. GNOME, KDE were written for Linux.

### Linux Goals

1. Be simple, elegant, and consistent. (Wildcards, cmd abbrs, etc.)
2. Power and flexibility. (every program just do one thing and do it well)
3. No redundancy.

## Processes in Linux

### Concepts

* One **PID(Process Identifier)** per process, and one **TID(Task Identifier)** per thread.
* **Child process** and **parent process** have their own memory images respectively, and share open files.
* **pipes**(which can be filled and then process blocked) and **signal**(`kill` cmd, for example) are used for inter-process communication.

### System Calls

* `fork` will create an exact duplicate of the original process(file descriptors, registers, and everything else), and applys the copy on write mechanism.
* `exec` will cause its entire core image to be replaced by the file named in its 1st parameter.
* `wait` is used to collect information and clean the zombie process left by the terminated child process.
* `pause` tells Linux to suspend the process until signal arrives.


### Thread

Kernel thread is supported by Linux using `clone` system call.

> When a thread was created, the original thread and the new one shared everything but their registers.

3 classes of threads for scheduling purposes:

1. Real-time FIFO.
2. Real-time round robin.
3. Timesharing.

### Booting

1. BIOS performs initial device discovery and initialization.
2. MBR is read into a fixed location and executed, which loads a standalone **boot** program like **GRUB(GRand Unified Bootloader)**.
3. `boot` reads in the OS kernel and jumps to it.
4. Process 0 checks devices, program the clock, mount the root FS, and create `init`(process 1), and page daemon(process 2).
5. `init` execs `/etc/rc/*`, finally opens tty and print `login:`

## Memory Management in Linux

### Concepts

* **text segement** lays in the lowest in virtual address space. It's readonly thus can be shared physically.
* **data segement** lays upside of text segment and contains 2 parts: initialized data and uninitialized data(called **BSS(block Started by Symbol)**, which can grow and shrink by system call `brk`).
* **stacks** starts at the top limit of virtual address. For a 32bit x86 platform, it would be 0xC0000000.
* **memory-mapped files** are often used for shared libraries.

> Text segment is read-only. Self-modifying programs went out of style in 1950s because they they were too difficult to understand and debug.

> The existence of uninitialized data is actually just an optimization to make binary programs smaller.

### Implementation

* Each linux process on a 32-bit machine typically gets 3GB of virtual address space for itself, with the remaining 1GB reserved for its page tables and other kerel data.
* The 1GB kernel memory typically resides in low physical memory but it's mapped in the top of each process virtual address space.
* Linux uses 4-level paging scheme
* **buddy algorithm** and **slab allocator** is used for memeory allocation for normal objects and kernel caches respectively.

## Input/Output in Linux

File systems under the VFS includes:

1. Regular file(with I/O scheduler and Block device driver)
2. Block special file(with I/O scheduler and Block device driver)
3. Char special file(with Optional line discipline and Char device driver)
4. Network socket(with Protocol drivers and Network device driver)

* I/O Scheduler is used to reorder or bundle r/w requests to block devices.
* Line disciplines are used to render the local line editing before submit by Carriage Return.
* Linux treat drivers as **loadable modules**.

## Linux File System

### Concepts

History of Linux FS:

1. The initial FS is MINIX 1 FS.
2. ext FS allowed 255 chars of filename and 2GB filesize, but slower.
3. ext2 allowed long filename, larger filesize, and provided better performance.
4. ext3 is a follow-on of ext2 with journaling.

Linux allows directory and file locking(byte range) with semaphore, including **shared locks** and **exclusive locks**.

### Implementation

4 main structures of VFS:

1. **superblock** contains critical information about the layout of the file system.
2. **i-nodes** each describe exactly one file.
3. **dentry** represents a directory entry.
4. **file** is an in-memory representation of an open file.

![ext2 layout](/assets/img/blog/10-31.bmp)

* With 1kb block, this design limits a block group to 8192 blocks(in practice) and 8192 inodes(not real restriction).
* Ext2 attempts to collocate ordinary files in the same block group as the parent directory, and data files in the same block as the original file i-node.
* Ext2 also preallocates a number(8) of additional blocks for that file to minimize the file fragmentation due to future write operations.
* inode contains addresses of first 12 disk blocks, single indirect, double indirect, and triple indirect when needed.


