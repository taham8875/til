# Chapter 13 - The Abstraction: Address Space

## Introduction - Stack vs Heap

The application memory can be divided into four sections:

- **Code**: The code section contains the program's executable code.
- **Static/Globals**: The static section contains global variables that are initialized by the programmer.
- **Stack**: The stack section contains the function call stack, local variables, and function parameters.
- **Heap**: The heap section contains dynamically allocated memory.

Note that the first three sections are fixed in size and can't grow or shrink at runtime. The heap, on the other hand, can grow or shrink at runtime.

## Early Systems

In early systems, machines didn't provide much of an abstraction to users. The os was a set of routines (libraries) that sat in memory (starting at physical address 0) and the running program, process, starts at fixed physical address.

## Multiprogramming and Time Sharing

After a while, machines became more expensive, and people wanted to share them more effectively, thus the era of multiprogramming was born, in which multiple processes were ready to run at a given time and the os would switch between them to increase the utilization of the cpu.

People began demanding more of machines and the era of time sharing was born, in batch computing has limitations specially on the programmers who where tired of ling program debug cycles, the interactivity became important as many users might be concurrently using the machine expecting a quick response.

One way to implement time sharing would be to run one process for a short while giving it access to all the memory, and then stop it, save aff of its state to disk, load some other memory state, run it for a while, and so on. This is called swapping.

Unfortunately, this approach has a big problem, it is so slow, specially when the memory grows, saving and restoring the memory state to disk is very slow. A better approach is to leave the process state in memory and switch between them, allowing the os to implement time sharing effectively.

As time sharing became more popular, the protection problem became more important, as the os has to protect the memory of one process from another, you don't want a process to be able to read or (worse) write the memory of another process.

## The Address Space

The solution is to create an easy to use abstraction of physical memory, called address space, and it is the running program's view of memory in the system.

The address space contains all of the memory state of the running program, code, stack and heap.

For now, when we describe the address space, what we are describing is the abstraction that the os provides to the running program, not the actual physical memory.

By using memory isolation, the os ensures that running programs cannot affect the operation of the underlying os, some modern os's takes it step further, by walling of pieces of the os from other pieces of the os, such micro kernels provides greater reliability than monolithic kernels.

## Goals

- **Transparency**: The os should provide the illusion that the program has all of the physical memory to itself.
- **Efficiency**: The os should provide this illusion efficiently, without too much time or space overhead.
- **Protection**: The os should protect the memory of one process from another.

In the next chapters, we will focus our exploration on the basic mechanisms needed to virtualize memory, and how the os uses these mechanisms to provide the illusion of a large, private address space to each running program.

> Remember and never forget that if you prints some memory address in a program, it is not the physical address, it is the virtual address, the os will translate this virtual address to a physical address.
