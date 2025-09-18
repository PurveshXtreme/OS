
---

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

**Differences between process and thread:**
- A **process** is a program in action, whereas a **thread** is a subset (segment) of instructions within a process that can be scheduled independently.
- Threads are lightweight and share the same address space (code, data, and OS resources like open files and signals) with other threads of the same process. However, each thread maintains its own program counter, register set, and stack, allowing for independent execution.
- Processes are fully independent entities, while threads are not and can communicate and synchronize more efficiently. This makes threads suitable for concurrent and parallel execution in multi-threaded environments.
