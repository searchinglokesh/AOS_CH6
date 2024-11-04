# Interprocess Communication (IPC) - Comprehensive Q&A Guide

### Q: What is Interprocess Communication (IPC)?
A: Interprocess Communication (IPC) refers to mechanisms that allow processes to communicate with each other and synchronize their actions. It's a fundamental aspect of operating system design that enables processes to share data and resources.

### Q: What are the main purposes of Interprocess Communication?
A: The main purposes of IPC are:
1. Data Transfer - Exchanging information between processes
2. Data Sharing - Allowing multiple processes to access shared data
3. Event Notification - Notifying other processes about specific events
4. Resource Sharing - Enabling processes to share system resources
5. Process Control - Allowing control over other processes, including trap and exception handling

## IPC Mechanisms

### Q: What are the universal IPC features available in most systems?
A: The two most universal IPC features are:
1. Signals
2. Pipes

### Q: What are signals and their limitations in IPC?
A: Signals are software interrupts sent to processes. While they can be used for synchronization (using sigpause), they have several limitations:
1. They are computationally expensive
2. They have very limited bandwidth for data sharing
3. They are best suited only for small information sharing
4. They are primarily useful for simple notifications rather than data transfer

### Q: What are pipes and how do they work?
A: Pipes are unidirectional FIFO (First In, First Out) channels with these characteristics:
- They provide an unstructured data stream of fixed maximum size
- Writers add data at the end of the pipe
- Readers retrieve data from the front of the pipe
- Once read, data is removed and unavailable to other readers
- They are typically shared between two processes

### Q: How does pipe handling work when pipes are empty or full?
A: Pipe handling is managed through blocking:
- When a process attempts to read from an empty pipe, it blocks until more data is written
- When a process attempts to write to a full pipe, it blocks until space becomes available

### Q: What are the limitations of pipes?
A: Pipes have several limitations:
1. Data cannot be broadcast to multiple receivers since reading removes data
2. No message boundaries are maintained
3. Cannot target specific readers when multiple readers exist
4. Limited to use between related processes (unless using named pipes/FIFOs)

## Advanced IPC Mechanisms

### Q: What are FIFOs and how do they compare to pipes?
A: FIFOs (named pipes) are special files that can be used for IPC. 

Advantages over regular pipes:
1. Can be accessed by unrelated processes
2. They are persistent
3. Have a name in the file system namespace

Disadvantages:
1. Must be explicitly deleted
2. Less secure than pipes
3. Require more resources than pipes

### Q: What is Process Tracing (ptrace)?
A: Process tracing is a debugging facility with these key features:
- Allows parent processes to control child execution
- Enables reading/writing of child process memory
- Permits interception of signals
- Controls child process execution (resume/stop)
- Syntax: `ptrace(action, child_pid, addr, data)`

### Q: What are the common elements in IPC mechanisms like semaphores, message queues, and shared memory?
A: Common elements include:
1. Key - Identifies specific IPC resource instances
2. Creator and Owner - User and group IDs of resource creator
3. Permissions - Access control based on read/write/execute flags
4. System calls for resource acquisition (shmget, semget, msgget)

### Q: What are semaphores and how do they work?
A: Semaphores are integer-valued synchronization primitives that support two atomic operations:
- P() Operation: Decreases value and blocks if result is negative
- V() Operation: Increases value and potentially unblocks waiting processes
- System V allows creation of semaphore arrays using semget

### Q: What is shared memory and why is it important?
A: Shared memory is a physical memory segment shared between processes. Key aspects include:
1. Fastest IPC method as it avoids system calls for read/write operations
2. Created using shmget(key, size, flag)
3. Attached using shmat(shmid, shmaddr, shmflag)
4. Detached using shmdt(shmaddr)
5. Deleted using shmctl(shmid, IPC_RMID)

### Q: How do message queues work?
A: Message queues are lists where processes can store and retrieve messages in FIFO order:
1. Each message has a 32-bit type and data area
2. Created/accessed using msgget(key, flag)
3. Messages sent using msgsnd(msgquid, msgp, count, flag)
4. Messages received using msgrcv()
5. Queues deleted using msgctl

## SVR4 Pipes

### Q: What are the special features of SVR4 pipes?
A: SVR4 pipes have several unique characteristics:
1. They are bidirectional
2. Built on the STREAMS framework
3. Can be attached to filesystem using fattach()
4. Support module pushing for additional functionality
5. Return two descriptors for read/write operations
6. Syntax: `status = pipe(int fildes[2])`

### Q: What security considerations exist for SVR4 pipes?
A: Key security aspects include:
1. Only non-privileged users can push modules to streams they own
2. Access control through file system permissions when attached
3. More secure than FIFOs for inter-process communication
