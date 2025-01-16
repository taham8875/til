# Chapter 6 - Mechanism: Limited Direct Execution

In order to virtualize the CPU, the os need to somehow share the physical CPU among multiple processes. This is done by running one process for a little while, then run another one, and so forth. by time sharing the cpu in this manner virtualization is achieved.

There are a few challenges, the first is performance, how can we implement virtualization without a significant performance hit? The second is control, how can we run processes efficiently while retaining control over the cpu? control is important to the os as it is in charge for resources, without control, a process could simply run forever and take over the machine, or access information that snot allowed to access.

## Basic Technique: Limited Direct Execution

To take a program run as fast as possible, os developers came up with "Limited Direct Execution" technique, the "direct execution" part of the idea is simple, just run the program directly on the cpu, when the os whishes to start a program running it creates a process entry in the process list, allocates some memory into it, loads the program code into memory form disk, allocates its entry point (i.e. `main()`) jumps to it and start running the code, the following figure shows the direct execution protocol: with normal call and return to jump to the program `main()` function and later to get back to the terminal.

![direct execution protocol](assets/direct-execution-protocol.png)

This approach introduces a few problems, the first is if we just run a program how can the os makes sure the program doesn't do anything that we don't want it to do while still running it efficiently? the second: when we are running a process, how does the operating system stop it from running and switch to another process thus implementing the time sharing technique?

# Problem 1 - Restricted Operations

Direct execution is fast, the program runs natively on the hardware cpu, but the problem is what if the process wishes to perform some kind of restricted operation, such as issuing an io request to a disk or gaining access to more resources such as cpu or memory? The process must be able to perform io and some other restricted operations but without giving the process the complete control over the system.

Teh hardware assets the os by provides different modes for execution:

- **User Mode**: In this mode the process is restricted to perform only a subset of operations, not a full access to the hardware resources
- **Kernel Mode**: In this mode the process has full access to the hardware resources

One approach is to simply be leave any process do whatever it wants in the terms of io operations and other related operations, however, doing so would prevent the construction of many systems that are desirable, for example, a system that checks permissions before granting access to a file.

The approach to take is to introduce a new process more known as user mode, the cond runs in the user mode is restricted in what it can do, for example, when running in user mode, a process can't issue io requests. doing so would result in the processor raising an exception and the os would likely terminate the process.

In contrast to the user mode there is the kernel mode, which the os (or the kernel) runs in, in this mode the os has full access to the hardware resources and can do whatever it wants, including executing all types of restricted operations.

We are still left with a challenge, what should a user program do when it needs to perform a restricted operation such as reading from the desk? to enable this, all modern hardware provides the ability for user programs to perform a system call, system calls allow the kernel to carefully expose certain key pieces of functionality to user programs, like accessing the file system, creating or destroying processes, communicating with other processes, and allocating more memory.

To execute a system call, a program must execute a special trap instruction. This instruction jumps to the kernel and raises the privilege level to kernel mode so the system can now perform whatever privileged operation the user program requested if allowed.

When done, the os calls a special return from trap instruction to return to user mode and the user program can continue running.

The hardware needs to be a bit careful when executing a trap, it must make sure to save enough of the caller's register state in order to be able to return correctly when the return from trap instruction is executed, for example, ion X86 the process will push the program counter, flags and a few other registers onto a pre process kernel stack, the return from flag will pop these values off the stack and resume execution.

how does the trap know what code to run inside the os?

- the calling process can't specify an address to jump to, doing so would allow the process to run any code it wants in the os (anywhere in the kernel), which is a bad idea.
- the kernel does so by setting up a trap table at boot time, when the machine boots up, it does so in privileged (kernel) mode, and thus is free to configure machine hardware as need be, one of the first things the os does is to tell the hardware what code to run when certain events occur, for example, when a hard disk interrupt takes a place, when a keyboard interrupt takes place, and when program makes a system call.

The os informs the hardware of the location of these trap handlers, usually with some kine of special instruction. Once the hardware is informed, it remembers the location of these handlers and until the machine is next rebooted, and thus the hardware knows where to jump when a system call happen or a specific interrupt occurs.

There are two phases in the LDE protocol, the first at boot time the kernel initializes the trap table and the cpu remembers its location for subsequent use.

In second (while running a process), the kernel sets up a few things (e.g. allocating node on the process list or allocating memory) before using a return from trap instruction to start the execution of the process.

This switches the cpu to the user mode and begins running the process, when the process want to make a system call, it trap back into the os which handles it and once again return control bia a return from trap to the process, the process then completes its work, return from the main() function, this usually will return into sum stub code which exits the program (calling `exit()` system call) and then the os will clean up the process and switch to another one.

## Problem 2 - Switching Between Processes

The next problem with direct execution is switching between processes, if a process is running on the cpu, this by definition means that os is not running, so it can't do anything, like switch to another process. The question is how the os can regain control so it can switch to another process?

## A Cooperative Approach: Wait for System Calls

One approach that some system have taken in the past is the cooperative approach, which means that the os trusts the processes of the system to behave reasonably, process that runs so long are assumed to periodically give up the cpu so that the os can decide to run another process.

You might ask how processes can be that friendly? most processes as it turns out transfer control of the cpu to the os quite frequently by making system calls, for example opening a file and reading it or sending a message to another machine etc. Systems like this ofter has explicit yield system calls that a process can make to give the cpu the control so it can run other processes.

Applications also transfer the control to the os when they do something illegal, for example, trying to divide by zero or access memory that doesn't belong to them, in these cases it will generate a trap to the os, the os will have the control of the cpu again (trying to terminate offending processes)

But this is not an ideal approach, what will happen if a buggy code has an infinite loop or don't have a system calls, what the os can do in this case?

## A Non Cooperative Approach: The OS Takes Control

How to take control if there is an infinite loop for example?

The answer turns out to be simple, a timer interrupt, a time device can be programmed to raise an interrupt every N milliseconds, when the interrupt occurs, the current running process is halted and a pre-configured interrupt handler in the os is run.

As we discussed before, the os must inform the hardware of which code to run when the timer interrupt occurs at the boot time.

Note that the os has some responsibility to make sure that when an interrupt occurs to save enough of the state of the program that was running such that a subsequent return from trap instruction be able to resume the program correctly.

## Saving and Restoring Context

Now the os has regain the control either by cooperative or non cooperative approach via timer interrupts, it nwo has to decide whether to continue running the current process or switch to another one. This decision is made by a part of the os called the scheduler, we will discuss this in the next chapters.

If the decision is to switch, the os then executes a low level code called (context switch), a context switch is conceptually simple, all the os has to do is save a few register values for the currently executing program,  (onto the kernel stack to save it) and restore a few for the new to execute process (from the kernel stack), by doing so the is is sure that when the return from trap instruction is executed, instead of returning to he process that was running, it will return to the new process.

## Worried About Concurrency?

You may ask what will happen if during a system call or time interrupt an another system call or time interrupt occurs? This is the exact topic we will discuss in the next chapters (Concurrency).

We will sketch some basics on how os handles these tricky situations, for example, the os might disable interrupts during interrupt processing to prevent another interrupt from occurring, or it might use a special data structure to keep track of all the processes that are currently running and switch between them.
