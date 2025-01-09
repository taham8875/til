# Chapter 4 - The Abstraction: The Process

the definition of the process, informally: *is a running program*, the program itself is a lifeless thing, it just sites there on the disk, a bunch of instructions and may be some static data, waiting to spring into action, the os takes these bytes and get them running, transforming this into something useful

It turns out that one often wants to run more than one program at once, a typical system may be seemingly running tens or hundreds of processes at the same time, so:

How to provide the illusion of many cpus, although there is only a few physical cpus available, how the os provides the illusion of endless supply of cpus?

The os creates this illusion by virtualizing the cpu, This basic technique (time sharing of cpu) allows user to run as many processes as possible, the potential cost is performance, as  each will run more slowly if the cpu must be shared.

To implement virtualization of the cpu, os will need both some low level machinery as well as some high level intelligence, we can call the low level machinery **mechanisms**; mechanisms are low level methods or protocols that implement a needed piece of functionality, for example, we will learn how to create a context switch, which gives the os the ability to stop running one program and start running another, this time sharing mechanism is employed by all modern operating systems.

On the top of these mechanisms resides some of the intelligence in the os, in the form of policies. policies are algorithms for miking some kind of decision in the os, for example, given the number of possible programs to run on a cpu, which program should the os run? a scheduling policy in the os will make the decision, likely using historical information (e.g. which program has rum more over the last minutes), or workload knowledge (e.g. which types of programs are running), and performance metrics, (is the system optimizing for interactive performance or throughput?) to make the decision.

> Time sharing is one of the most basic techniques used by the os to share a resource, by allowing the resource to be used for a little while by one entity, and then a little while by another ans so forth, the resource in question, (e.g. the cpu or a network link) can be shared by many, the natural counterpart of time sharing is space sharing, where a resource is divided in space among those who wish to use it, for example, a disk space is naturally a space shared resource, as one block is assigned to a file, it is not likely to be assigned to another file until the user deletes it.

## The Abstraction: The Process

The abstraction provided by the os of a running program is something we call a process, process is simply a running program; at any instant in time, we can summarize a process by taking inventory of the different pieces of the system it accesses it or affecting during the execution.

To understand what constitutes a process, we need to understand the machine state, what a program can read or update when it is running at any given time, what parts of the machine are important to the execution of the program?

One component of the machine state that comprises a process is its memory, instructions lie in memory and the data the running process read or writes sits in memory as well. Thus the memory that the process can address (called it address space) is part of the process.

Also part of the process machine state are registers, many instructions read or update the registers thus it is clearly important to the process.

An example of some special registers that form part of the machine state are program counter (PC) (sometimes called instruction pointer or IP), which points to the next instruction to be executed, similarly the stack pointer and associated frame pointer are used to manage the stack for function parameters, local variables and return addresses.

Finally, programs ofter access persistent storage devices, such io information might include a list of the files the process currently has open.

## Process API

We will give some idea of what mush be included in any interface of an os, these apis in some form are available in any modern os:

- **Create**: to create a new process, for example when you type a command in the shell of double click an icon in a windowing system, the os must create a new process to run the program.
- **Destroy**: to destroy a process, when a process is done running, the os must clean up after it, releasing any resources it was using.
- **Wait**: to wait for a process to finish, sometimes a process needs to wait for another process to finish, for example, a parent process may need to wait for a child process to finish.
- **Miscellaneous control**: to provide other control over processes, for example, to suspend a process for a while and the ability to resume a process.
- **Status**: to provide status information about a process, for example, (the state of it like) how much cpu time has it used, how much memory is it using or for how long has it been running.

## Process Creation: A Little More Detail

How does an os get a program up and running? how the process creation actually works?

- The first thing the os must do to run a program is to load its code and any static data (like initialized variables) into memory. Programs initially reside on disk, (or in some modern systems SSDs), in some kine of executable format; thus, the process of loading ta program ans its static data into memory requires the os to read those bytes from disk and place them into memory.

In early or simple systems, the loading process is done eagerly, i.e. all at once before running the program; modern OSes perform this process lazily (load only needed code or data)

Once the code and static data are loaded into memory, there are a few other things the os needs to do before running the process, some memory must be allocated for the program's stack, the os will also likely to initialize the stack with arguments, specifically, it will fill in the parameters to the `main()` function, i.e. `argc` and `argv` array.

The OS may also create some initial memory for the program's heap, in C programs, the heap is used for explicitly requested dynamically allocated data; programs request such space by calling `malloc()` and free it by calling `free()`. The heap is needed for data structures like linked lists, hash tables, heaps, ... etc. The heap will be small as first, as the program runs and requests more memory via `malloc()` the os may get involved and allocate more memory to the process to aid such calls.

The os will also do some other initialization tasks, particularly related to the input/output i/o, for example, in unix systems, each process by default has three open file descriptors, for standard input, standard output and standard error, these descriptors let programs easily read input form the terminal as well as print output to the screen, we will learn more about io and file descriptors in the persistence part.

By loading the code and the static data into the memory, by creating and initializing the stack and by doing other work like io setup, the os now has finally set the stage for program execution, it has one last task, to start the program running at the entry point, mainly the `main()`. By jumping to the main routine, the os transfers the control of the cpu to the newly created process and start the execution of the program.

## Process State

Now that we have an idea of what a process is, and how it is created, let's talk about the different states a process can ve in at a given time, the process can be in one of three states:

- Running: In the running state, a process is executing instructions
- Ready: In the ready state, a process is ready to run but for some reason the IS has chosen not to run it at this given environment.
- Blocked: A process has performed some kind of operation that makes it not ready to run until some other event takes place, for example, a process may be blocked waiting for a disk io operation to complete.

## Data Structures

the OS is a program, and like any program it has some key data structures that track various relevant pieces of information, to track the state of each process for example, the os may keep a **list** for all processes that are ready, as well as some additional information to track which process is currently running, the blocked process, the process ready to run again when an io operation completes and so on.

Beyond running, ready and blocked there are some other states a process can be in, for example, a process may be in the **zombie** state, which is a state that occurs when a process has finished executing but the os has not yet cleaned up after it. Or an **initial** state when it is created, or a **final** state when it is has exited but has not been cleaned up (the zombie state).

The final state can be useful as it allows other processes (usually the parent that created the process) to examine the return code of the process and see if the just finished process executed successfully or not. (in unix based systems, return zero means success, non-zero means failure).

When finished, the parent will make one final call (e.g. `wait()`) to wait for the completion of the child, and to also indicate tot he os that it can clean up any relecant data structures that referred to the child process.
