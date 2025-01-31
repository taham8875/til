# Chapter 17 - Free Space Management

In this chapter, we will discuss ho to manage free space in the memory, how to satisfy variable sized requests to allocate memory to processes, how to minimize fragmentation, and how to coalesce free space so that it can be used to satisfy larger requests.

Remember **external fragmentation**: the free space gets chopped into little pieces of different sized and thus get fragmented, subsequent requests may fail because there is no single contiguous block of memory that is large enough to satisfy the request, even though the total free space is enough.

## Assumptions

Most of this discussion will focus on the history of allocators found in user-level memory-allocation libraries.

We assume a basic interface such as the provided by `malloc()` and `free()` in C.

`void *malloc(size_t size);` takes a single parameter, the number of bytes to allocate, and returns a pointer to the start of the allocated memory of type `void *`.

`void free(void *ptr);` takes a single parameter, a pointer to the start of the memory to free.

Note that the user when freeing the space doesn't need to specify the size of the memory to free, the allocator should be able to figure that out as we will see later.

The space that this library manages is known as the **heap**. And the data structure used to manage free space in the heap is known as the **free list**, free list contains references to the free blocks of memory.

We will primary focus on **external fragmentation**, there are another type of fragmentation known as **internal fragmentation**, which happens when the allocator hands out chunks of memory that are larger than requested, and the extra space is unused and therefore wasted, the difference between this and external fragmentation is that in internal fragmentation the waste occurs inside the allocated block.

We will also assume that once the memory is handed to a user, it is owed by him and cannot be relocated to another location in memory, therefore there are no compaction operations.

We will also assume that the memory region is single fixed size throughout the lifetime of the program, this is not true in reality as in some cases the allocator can request more memory from the OS and extend the heap. (via a system call like `sbrk()`)

## Low Level Mechanisms

### Splitting and Coalescing

The free list is a data structure that contains references to the free blocks of memory, the allocator uses this data structure to find a block of memory that is large enough to satisfy a request.

Example of 30 bytes of heap memory:

```
[free from 0 to 9] [used from 10 to 19] [free from 20 to 29]
```

Example of free list

```
head -> [addr: 0, len: 10] -> [addr: 20, len: 10] -> NULL
```

The request for anything bigger than 10 bytes will fail, (return NULL), as there is no single contiguous block of memory that is large enough to satisfy the request even though the total free space is enough. A request for 10 bytes will succeed.

but what will happen for a request less that 10 bytes? the allocator can split the block of 10 bytes into two blocks, one of the requested size that returns to the caller and the other of the remaining size.

If a request of 1 byte is made and the allocator decided to use the second of teh two elements on the list to satisfy the request, the free list will be updated to remove the block that was used.

```
head -> [addr: 0, len: 10] -> [addr: 21, len: 9] -> NULL
```

Another mechanism in allocators is coalescing, when a block of memory is freed, the allocator can check if the adjacent blocks are also free and coalesce them into a single block to satisfy larger requests.

Now lets free all the memory used in the previous example:

The free list will be updated to contain three blocks of free memory.

```
head -> [addr: 0, len: 10] -> [addr: 20, len: 10] -> [addr: 30, len: 10] -> NULL
```

After coalescing the free list will be updated to contain a single block of free memory.

```
head -> [addr: 0, len: 30] -> NULL
```

### Tracking The Size of Allocated Regions

You can notice that the interface of `free()` doesn't take a size parameter, thus it is assumed that the allocator should be able to figure out the size of the memory to free and hand it back to the free list.

To accomplish this task, most allocator stores a little bit of extra information in the header block which is kept in the memory, usually just before the handed out chunk of memory.

The header minimally contains the size of the allocated region (in this case, 20); it may also contain additional pointers to speed up deallocation, a magic number to provide addition integrity checks, and so on. So when you call `free`, the allocator can look at the header, figure out the size of the block, and then add it back to the free list. And when the user requests a free chunk of memory, the user will be given a pointer to the request memory + the size of the header.

## Basic Strategies

### Best Fit

The best-fit strategy is to search through the free list and find the smallest block that is large enough to satisfy the request, this strategy minimizes the amount of wasted space in the heap.

Can be called smallest fit too.

However, this strategy is slow as it requires searching through the entire free list to find the best fit.

### Worst Fit

The worst-fit strategy is to search through the free list and find the largest block that is large enough to satisfy the request, this aims to leave big chunks free instead of lots of small chunks that can arise from the best-fit strategy.

Like the best-fit strategy, this strategy is slow as it requires searching through the entire free list to find the worst fit. And many studies have shown it performs badly

### First Fit

The first-fit strategy is to search through the free list and find the first block that is large enough to satisfy the request, this strategy is faster than the best-fit and worst-fit strategies as it requires searching through the free list until the first block that is large enough is found.

### Next Fit

The next-fit strategy is similar to the first-fit strategy, but instead of starting the search from the beginning of the free list, it starts the search from the last block that was used to satisfy a request.

## Other Approaches

### Segregated Lists

The segregated lists strategy is to maintain multiple free lists, if a particular application has one or fre popular sized requests that it makes, the allocator can maintain a separate free list for each of these sizes.

This strategy is useful when the application makes a lot of requests of the same size, as it can reduce the time required to find a block that is large enough to satisfy the request.

### Buddy Allocation

The buddy allocation strategy is to maintain a binary tree of free blocks, each block is split into two equal-sized buddies, and when a block is allocated, the buddy is marked as used.

When a block is freed, the allocator can check if the buddy is also free and coalesce them into a single block.

This strategy is useful when the application makes a lot of requests of power-of-two sizes, as it can reduce the time required to find a block that is large enough to satisfy the request. But it can lead to internal fragmentation.
