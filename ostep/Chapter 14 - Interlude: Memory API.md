# Chapter 14 - Interlude: Memory API

In this interlude, we discuss the memory allocation interfaces in unix systems, the interface provided are quite simple, the main problem is this: How to allocate and manage memory?

## Types of Memory

In a running C program, there are two types of memory that are allocated, the first is the **stack** and the second is the **heap**.

The stack memory allocation and deallocation are managed by the compiler for you, so it is sometimes called automatic memory.

Declaring memory on the stack in c is easy, you just create a function and declare a variable, the memory is allocated when the function is called and deallocated when the function returns. If you need some information to live beyond the call invocation, you had better not to leave it on the stack. This introduces us to the second type of memory, the heap, where all allocation and deallocations are explicitly handled by you, the programmer.

To allocate memory on the heap, you use the `malloc` function, and to deallocate memory, you use the `free` function.

```c
void func() {
    int *x = (int *) malloc(10 * sizeof(int));
    // use x
    free(x);
}
```

You might notice that both allocation of stack and heap happened in the same line, the compiler needed to make roon for a pointer to an integer when it sees your declaration, when the code calls `malloc` it requested space for and integer on the heap, the routine returns the address of such an integer (upon success, or `NULL` upon failure), and the address is stored in the variable `x` on the stack.

Because of its explicit nature, and because of its more varied usage, heap memory presents more challenges to both users and systems, thus, it is the focus of the remainder of our discussion.

## The `malloc` call

The `malloc` call is quite simple, you pass it a size asking for some room on the heap, and it either succeeds and gives you back to a pointer to the newly allocated space, or it returns `NULL` if it fails.

The single parameter `malloc` takes is of type `size_t` which describes how many of bytes you need, however, most programmers do not type directly the number of bytes, instead, they use the `sizeof` operator to determine the size of the type they are allocating.

```c
double *d = (double *) malloc(sizeof(double));
```

> `sizeof` is a compile-time operator, it is not evaluated at runtime, it is evaluated by the compiler and replaced with the size of the type. Dislike other functions, `sizeof` is not a function, it is an operator.

You might also pass in the name of a variable (and not just the type) to `sizeof`, but in some cases you may not get the desired results, so be careful.

```c
int *x = malloc(10 * sizeof(*int));
printf("%d\n", sizeof(*x));
```

In the first line, we have declared space for an array of 10 integers, which is fine, however, when we use `sizeof()` in the next line, it returns a small value such as 4 or 8, the reason is that `sizeof` thinks we are simply asking how big a pointer to an integer is, not how much memory we have allocated.

But sometimes, `sizeof()` does work as you might expect:

```c
int x[10];
printf("%d\n", sizeof(x));
```

In this case, there is enough information for the compiler to know that 40 bytes have been allocated.

Another place to be careful with is strings, when declaring a space for a string, use the following: `malloc(strlen(s) + 1)`, the `+ 1` is for the null terminator, using `sizeof` will not work in this case.

Note that `malloc` returns a pointer of type void, doing so let's the programmer to cast the pointer to any type they want, casting doesn't really do anything, other than telling the compiler and other programmers who might read your code that you know what you are doing. Casting is for reassurance, not for the correctness of the code.

## The `free` call

The `free` call is quite simple, you pass it a pointer to the memory you want to deallocate, and it will return the memory to the heap.

```c
int *x = (int *) malloc(10 * sizeof(int));
// use x
free(x);
```

This routine takes one argument, a pointer to the memory you want to deallocate, and it does not return anything.

## Common Errors

Correct memory management has been a problem, in fact, many newer languages have support for automatic memory management (garbage collection), in such language, when you call something like `malloc`, the garbage collector runs and figure out when the memory is no longer needed and frees it for you.

### Forgetting to Allocate Memory

Many routines expect the memory to be allocated before you call the, for example, the `strcpy(dst, src)` function expects that `dst` is a pointer to a buffer that is large enough to hold the string in `src`, if you forget to allocate memory for `dst`, you will get a segmentation fault.

```c
char *src = "hello";
char *dst; // oops! unallocated memory
strcpy(dst, src); // segmentation fault
```

Segmentation fault is a fancy term for "you did something wrong with memory".

In this case, the proper code might instead look like this:

```c
char *src = "hello";
char *dst = (char *) malloc(strlen(src) + 1);
strcpy(dst, src);
```

Alternatively, you could use `strdup` which does the same thing as the above code, read the friendly manual for more information.

### Not Allocating Enough Memory

A related error is not allocating enough memory, sometimes called a buffer overflow, in the example above, you might make an error by allocating *almost* enough memory, but not quite.

```c
char *src = "hello";
char *dst = (char *) malloc(strlen(src)); // too small
strcpy(dst, src); // buffer overflow
```

Oddly enough, this code might work, depending on how `malloc` is implemented and many other details, this program will often run successfully, but in some cases, when the string copy get executed, it writes one byte too far past the end of the allocated space, may be harmless if it overrides a variable that no longer needed, but it could also overwrite some important data, causing the program to crash.

buffer error is the cause of many security vulnerabilities, and it is a common problem in C programs. Sometimes `malloc` allocates extra space anyhow, so your program does not mess with other data, but you should not rely on this behavior.

### Forgetting to Initialize Allocated Memory

With this error, you may call `malloc` properly, but forget to fill in some values into your newly allocated data type, if you do so, your program will encounter an **initialized read**, where it reads from the heap some data of unknown value, who know what can be in there, if you get lucky, it might be zero, if you are not, it might be some random and harmful value.

### Forgetting to Free Memory

Another common error is a **memory leak**, where you allocate memory but forget to free it, in some long running applications such an os, this is a huge problem, as slowly leaking memory will eventually cause the system to run out of memory. At that point a restart is required.

Note that not all memory need to be freed, at least, in certain cases. For example, when you write a short lived program, yoy might allocate some space using `malloc`, but if the program is going to complete and exit, do you need to call `free`? The answer is no, the os will reclaim all the memory when the program exits, so you can skip the call to `free`.

### Freeing Memory Before You Are Done With It

Sometimes a program will free memory before finishing using it, such a mistake is called a **dangling pointer**, a subsequent use can crash the program or overwrite some other data, when you call `malloc` again after calling `free`.

### Freeing Memory Repeatedly

Also known as **double free** happens when you free some memory more than once, the result of doing this is undefined and the memory allocating library might get confused and causes crashes.

### Calling `free` Incorrectly

`free` expects you to pass it a pointer that you received from `malloc`, if you pass it a pointer that you did not get from `malloc`, bad things can happen, **Invalid free** should be avoided.

## Underlying OS Support

You might have notices we didn't talk about system calls while discussing `malloc` and `free` in this chapter, the reason is that these routines are not system calls, they are library routines, these functions are built on top of some system calls which call into the os to ask for more memory or release memory to the os.

Examples of these system calls are `brk` and `sbrk`, but should never directly call these system calls as you will do horrible things, instead, you should use `malloc` and `free`.

## Other Calls

- **calloc**: Allocates memory and initializes it to zero, to avoid the uninitialized read problem.
- **realloc**: Resizes a previously allocated block of memory, if you need more space, you can call `realloc` to get more space, it will give you larger region of the memory.
