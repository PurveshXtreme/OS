## Table of Contents

- [Processes, Threads & Scheduling](#processes-threads--scheduling)
- [CPU Scheduling Algorithms](#cpu-scheduling-algorithms)
- [Memory Management & Virtual Memory](#memory-management--virtual-memory)
- [Deadlocks & Concurrency](#deadlocks--concurrency)
- [OS Basics](#os-basics)
- [Special Processes](#special-processes)

---

### Processes, Threads & Scheduling

### Q1: What is a process, process table, and what are the different states of a process?

A **process** is an instance of a program in execution. For example, a web browser or a command prompt running on your computer is a process. The operating system is responsible for managing all active processes, allocating resources such as processor time, memory, and disk access. To keep track of every process, the OS maintains a special data structure called the **process table**. This table contains entries for each process, recording information like the resources it uses and its current state.

A process can exist in one of several states:
- **Running:** The process has all required resources and is currently being executed by the processor. Only one process can be in this state on a single-core CPU.
- **Ready:** The process is prepared to run but is waiting its turn for the CPU.
- **Waiting (Blocked):** The process cannot continue until some external event occurs (e.g., completion of disk I/O or user input).

Typically, the OS organizes processes in the ready and waiting states into queues for efficient management and scheduling.

---

### Q2: What is a thread and how does it differ from a process?

A **thread** is the smallest unit of execution within a process, sometimes called a lightweight process. Threads allow applications to perform tasks in parallel, improving efficiency. For example, in a browser, each tab may run as a separate thread; in MS Word, one thread formats text while another processes user input.

### Differences between Process and Thread

| Process                                   | Thread                                                  |
|--------------------------------------------|---------------------------------------------------------|
| Program in execution; independent entity   | Schedulable unit within a process ("lightweight process")|
| Has its own address space and resources    | Shares address space and resources with other threads    |
| Communication between processes is complex | Threads can easily communicate and synchronize           |
| Context switching is more costly           | Faster context switching, efficient resource sharing     |


### Differences between Process and Thread

- A **process** is a program in execution, while a **thread** is a smaller, schedulable unit of execution within a process.
- Threads are considered "lightweight processes" because they share the same address space (code, data, and operating system resources like open files and signals) with other threads of the same process.
- Each thread has its own program counter, register set, and stack, allowing independent execution within the same process.
- Processes are fully independent entities; threads are not. Threads within the same process can easily communicate and synchronize with each other.
- Threads are more suitable for concurrent and parallel execution in multi-threaded environments because they enable faster context switching and efficient resource sharing compared to processes.


### Difference between User-Level Thread and Kernel-Level Thread

| User-Level Thread                                         | Kernel-Level Thread                                      |
|-----------------------------------------------------------|----------------------------------------------------------|
| Implemented by users                                      | Implemented by the operating system                      |
| Not recognized by the operating system                    | Recognized by the operating system                       |
| Easy to implement                                         | Complicated to implement                                 |
| Context switch time is less                               | Context switch time is more                              |
| No hardware support required for context switch           | Hardware support is needed for context switch            |
| If one thread performs a blocking operation, the entire process is blocked | If one thread performs a blocking operation, other threads can continue execution |
| Designed as dependent threads                             | Designed as independent threads                          |
---

## 1. What is Context Switching?

**Context Switching** is the process by which the operating system saves the state (context) of a currently running process or thread, so that it can resume execution at a later point, and loads the saved state of another process or thread to run it on the CPU. This is essential for multitasking environments, where multiple processes or threads need to share the CPU.  
During context switching:
- The current process's state (program counter, registers, etc.) is saved in its **Process Control Block (PCB)**.
- The PCB of the next process to run is loaded, restoring its previously saved state.
- The CPU starts executing the new process from where it left off previously.

This mechanism ensures that processes can be suspended and resumed without loss of information, providing the illusion of concurrent execution.

---

## 2. What is a Process Control Block (PCB)?

A **Process Control Block (PCB)** is a data structure maintained by the operating system to store all the information about a process. The PCB acts as a repository of the process's state, allowing the OS to manage and schedule processes efficiently.  
Key information stored in a PCB includes:
- **Process State:** Running, Ready, Waiting, etc.
- **Process Number:** Unique identifier (PID).
- **Program Counter:** Address of the next instruction to execute.
- **CPU Registers:** Contents of all process-specific registers.
- **Memory Management Information:** Pointers to the process's address space, page tables, etc.
- **Scheduling Information:** Process priority, scheduling queue pointers, etc.
- **Accounting Information:** CPU used, time limits, job or process numbers.
- **I/O Status Information:** List of I/O devices allocated to the process, open files, etc.

The **Process Table** is essentially an array (or list) of all PCBs, representing all processes currently tracked by the system.

---

## 3. What is the difference between preemptive and non-preemptive scheduling?

| Preemptive Scheduling | Non-Preemptive Scheduling |
|----------------------|--------------------------|
| The OS can interrupt a running process and allocate the CPU to another process (usually one with higher priority or shorter burst time). | Once a process gets the CPU, it runs until it terminates or switches to waiting state (e.g., for I/O). No interruption by OS. |
| Allows flexible handling of critical or high-priority processes. | More rigid, as running processes are not disturbed. |
| Overhead due to frequent context switches, maintaining ready queue, and switching process states. | No overhead of context switching between running and ready states. |
| Possible process starvation if high-priority processes keep arriving. | Possible starvation for short processes if a long process occupies CPU. |
| Must ensure data integrity and synchronization due to interrupts. | Less complex data integrity issues since processes are not preempted. |
| Example algorithms: Round Robin, Shortest Remaining Time First (SRTF), Priority Scheduling (preemptive). | Example algorithms: First-Come, First-Served (FCFS), Shortest Job First (SJF), Priority Scheduling (non-preemptive). |

---

## 4. What are starvation and aging in OS?

**Starvation:**  
Starvation is a situation in process scheduling where a process waits indefinitely for resources (CPU, memory, I/O), because the resources are continuously allocated to other processes. This often occurs in priority-based scheduling algorithms if lower-priority processes are always bypassed by higher-priority ones.

**Aging:**  
Aging is a technique used to prevent starvation. It gradually increases the priority of a process the longer it waits in the system. By doing so, even low-priority processes will eventually become high-priority and get scheduled, ensuring no process is starved forever.  
For example, every time unit a process spends waiting, its priority can be incremented, thus guaranteeing eventual CPU allocation.

---
---

### CPU Scheduling Algorithms

## Different CPU Scheduling Algorithms (Brief Explanation)

- **First-Come, First-Served (FCFS) Scheduling:**  
  The simplest scheduling algorithm. Processes are executed in the order they arrive in the ready queue. Non-preemptive and can lead to long waiting times for short processes if a long process arrives first.

- **Shortest-Job-Next (SJN) / Shortest Job First (SJF) Scheduling:**  
  The process with the shortest estimated burst time is selected next. Non-preemptive. Minimizes average waiting time but requires knowledge of future burst times.

- **Priority Scheduling:**  
  Each process is assigned a priority. The scheduler selects the process with the highest priority (usually the lowest numerical value). Can be preemptive or non-preemptive. May cause starvation for lower-priority processes.

- **Shortest Remaining Time (SRT) Scheduling:**  
  Preemptive version of SJN/SJF. If a new process arrives with a shorter remaining time than the currently running process, the CPU switches to the new process.

- **Round Robin (RR) Scheduling:**  
  Each process is assigned a fixed time slice (quantum) and processes are cycled through the ready queue. Preemptive, fair, and suitable for time-sharing systems.

- **Multiple-Level Queues Scheduling:**  
  The ready queue is split into several queues based on process types (e.g., system, interactive, batch). Each queue may use a different scheduling algorithm and have different priorities. Movement between queues can be restricted or allowed.

---
---

### Memory Management & Virtual Memory

## Memory Management & Virtual Memory – Structured Notes (Interview Perspective)

### What is Thrashing? When does it occur?
- **Thrashing** is a situation where the computer’s performance drops drastically because the system is spending more time handling page faults than executing actual processes.
- It happens when processes frequently access memory pages that are not in physical memory, causing constant loading and unloading from disk (paging device).
- As page fault rate increases, the queue for the paging device grows, increasing service time and reducing overall system performance.
- **When does thrashing occur?**  
  - Thrashing occurs when the system does not have enough physical memory to handle the active processes, leading to frequent page faults as processes access pages not currently in memory.

---

### What is Virtual Memory?
- **Virtual memory** is a memory management technique that gives an application the illusion of having a large, contiguous address space, even if the physical RAM is smaller.
- It works by using disk space to extend RAM, allowing processes to run without concern for whether data is in RAM or disk.
- Virtual memory is divided into small pieces (pages), which are loaded into physical memory as needed.

---

### What is Demand Paging?
- **Demand paging** is a technique where pages are loaded into physical memory only when they are needed (i.e., when a page fault occurs).
- This allows for efficient use of memory and enables running larger programs with limited physical memory.

---

### Difference between Logical and Physical Address Space

| Parameter    | Logical Address                                 | Physical Address                                 |
|--------------|-------------------------------------------------|--------------------------------------------------|
| Basic        | Generated by the CPU (virtual address)          | Actual location in memory unit                   |
| Address Space| All logical addresses generated by CPU for a program | All physical addresses mapped to logical addresses |
| Visibility   | User can view logical address                   | User cannot view physical address                |
| Generation   | Generated by CPU                                | Computed by Memory Management Unit (MMU)         |
| Access       | User uses logical address to access physical address | User cannot directly access physical address     |

---

### 24. What is Fragmentation?
- **Fragmentation** occurs when free memory is divided into small blocks and cannot be used for new processes due to their insufficient size.
- It is a common problem in dynamic memory allocation systems, resulting in wasted memory that cannot satisfy allocation requests.

---

### 25. What is the Basic Function of Paging?
- **Paging** is a memory management technique for non-contiguous memory allocation.
- Both main and secondary memory are divided into equal fixed-size partitions called **frames** (main memory) and **pages** (secondary memory).
- Paging allows processes to be loaded in parts (pages) into available frames, improving memory utilization and eliminating external fragmentation.

---

### 81. Difference Between Internal and External Fragmentation

| Internal Fragmentation                                   | External Fragmentation                                   |
|----------------------------------------------------------|----------------------------------------------------------|
| Fixed-size memory blocks are allocated to processes.      | Variable-sized memory blocks are allocated to processes.  |
| Occurs when memory allocated is larger than required.     | Occurs when processes are removed, leaving small free spaces. |
| Best-fit block is a solution.                            | Compaction, paging, and segmentation are solutions.      |
| Happens in fixed-sized partitions.                       | Happens in variable-sized partitions.                    |
| Difference between allocated and required memory.         | Unused small spaces between non-contiguous memory blocks.|

---

### 84. Difference Between Paging and Segmentation

| Paging                                                | Segmentation                                            |
|-------------------------------------------------------|---------------------------------------------------------|
| Program divided into fixed-size pages.                | Program divided into variable-size segments.            |
| Managed by operating system.                          | Managed by compiler.                                    |
| Page size decided by hardware.                        | Segment size given by user.                             |
| Faster than segmentation.                             | Slower than paging.                                     |
| Can cause internal fragmentation.                     | Can cause external fragmentation.                       |
| Logical address: page number + page offset.           | Logical address: segment number + segment offset.        |
| Uses page table to store base addresses.              | Uses segment table to store segment info.               |
| Maintains free frame list.                            | Maintains list of memory holes.                         |
| Invisible to user.                                    | Visible to user.                                        |
| Needs page number and offset for absolute address.    | Needs segment number and offset for absolute address.   |

---

### 95. What is Belady's Anomaly?
- **Belady's Anomaly** refers to the counterintuitive situation where, under certain page replacement policies (especially FIFO), increasing the number of page frames can actually increase the number of page faults.
- This anomaly highlights inefficiencies in some page replacement algorithms.

---
---
