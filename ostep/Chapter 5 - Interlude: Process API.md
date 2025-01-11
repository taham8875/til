# Chapter 5 - Interlude: Process API

> Interludes will cover more practical aspects of systems, including a focus on os apis and how to use them.

In this interlude we discuss process creation in unix systems, unix presents one of the most intriguing ways to create a new process with a pair of system calls, `fork()` and `exec()` and `wait()`, `wait()` can be used to wait for a child process to finish.

> How to create and control processes?
>
> What interface should the os present for process creation and control?

## The `fork()` System Call

The `fork()` system call is used to create a new process, let's take a look at the following code snippet:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char const *argv[]) {
    printf("Hello World (pid:%d)\n", (int)getpid());
    int rc = fork();
    if (rc < 0) {
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) {
        printf("Hello, I am child (pid:%d)\n", (int)getpid());
    } else {
        printf("Hello, I am parent of %d (pid:%d)\n", rc, (int)getpid());
    }
    return 0;
}
```

When you run the program you see the following output:

```bash
$ gcc -o main main.c && ./main
Hello World (pid:1780082)
Hello, I am parent of 1780083 (pid:1780082)
Hello, I am child (pid:1780083)
```

Let's understand what happened here:

- first when started running, the process prints out a hello world message, the message includes the process identifier, also known as pid, the pid is used to name the process to do some operations on it like killing it.
- then the  process calls the `fork()` system call, which the os provides as a way to create a new process. The odd part is the process creates is almost an exact copy of the calling process, that means that to the os it now looks like there are two copies of the program running and both are about to return from the `fork()` system call. The newly created process (the child) does not start running at `main()`. (as you may noticed, the hello world message is printed out one), rather, it just comes to life as `fork()` is called.
- you might have noticed, the child is not an exact copy, specifically, although it has its own copy of the address space (its own private memory), it own registers and its own pc and so forth, the value it returns form `fork()` is different.
- when the parent receives the pid of the newly created child, the child is simply returned a 0. This differentiation is useful, as it is simple then to write the code that handles the two different cases as above.
- you might also have notices, the output is not deterministic, when the child process is created, there are now two active processes in the system that we care about, the parent and the child
, assuming we are running on a system with a single cpu for simplicity, then either the child or the parent might run at that pint, in the example, the parent ran first, but it could have been the child.
- the cpu scheduler, (a topic we will discuss later) determines which process runs at any given time, because the scheduler is complex we can't give a strong assumption about what will run first or it will choose to do, the non-determinism leads to some interesting problems particularly in multi-threaded programs. we will see a lot of non-determinism in the concurrency part of the book.

## Adding `wait()` system call

it is quite useful for a parent to wait for a child process to finish what it has been doing. this task is accomplished by the `wait()` system call (or its more complete sibling `waitpid()`), let's take a look at the following code snippet:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char const *argv[]) {
    printf("Hello World (pid:%d)\n", (int)getpid());
    int rc = fork();
    printf("rc: %d\n", rc);
    if (rc < 0) {
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) {
        printf("Hello, I am child (pid:%d)\n", (int)getpid());
    } else {
        int rc_wait = wait(NULL);
        printf("Hello, I am parent of %d (rc_wait:%d) (pid:%d)\n", rc, rc_wait, (int)getpid());
    }
    return 0;
}
```

output:

```bash
$ gcc -o main main.c && ./main
Hello World (pid:97155)
Hello, I am child (pid:97156)
Hello, I am parent of 97156 (rc_wait:97156) (pid:97155)
```

In this example, the parent process calls `wait()` to delay its execution until the child finished executing, when the child is done, `wait()` returns to the parent.

Adding the `wait()` to the code made it deterministic and we know for sure that the child will always print first, how we know that? It might simply run first, as before and this print before the parent, however, if the parent does happen to run first, it will call `wait()` and this system call won't return until the child has run and exited, thus event if the parent runs first, it waits for the child to finish running before it prints out its message.

## The `exec()` System Call

This system call is useful when you want to run a program that is different from the calling program, for example, `fork()` is only useful if you want a copy of the same program, often you want to run a different program, `exec()` just does that.

> There are six variants of the `exec()` system call, `execl()`, `execle()`, `execlp()`, `execv()`, `execvp()`, read the friendly manual for more information.

In the following example, the child process calls `execvp()` in order to run the program `wc`, which is the word counting program, it runs the `wc` on our `main.c` file to tell us how many lines, words, and bytes are in the file.

```c
Hello World (pid:196444)
Hello, I am child (pid:196445)
 24 105 852 main.c
Hello, I am parent of 196445 (rc_wait:196445) (pid:196444)
```

What it does? given the name of  of an executable (e.g. `wc`) and some arguments (e.g. `main.c`), it doesn't create new process, it transforms the currently running program `main.c` to another program `wc`, after the `exec` call, the `wc` program is running, and the `main.c` program is gone and a successful `exec` call never returns.

When running `exec` it loads code and static data and overwrite its current code segment with it, the heap and stack and other parts of the memory space of the program are initialized then the os simple run the program.

## Why? Motivating the api

The separation of `fork()` and `exec()` is essential in building a unix shell, because it lets the shell runs code after the call to `fork()` but before the call to `exec()`this cade can alter environment of the about to run program and thus enables a variety of interesting features.

The shell is just a user program, it show you a prompt and then waits for you to type something into it, you then type a command (your executable program plus any arguments) into it, in most cases the shell figures out where in the file system does you executable exist, calls `fork()` to create a new process, and then calls `exec()` to run the program and then waits for command to complete by running `wait()`, when the child completes, the shell returns from `wait()` and prints out the prompt again ready for the next command.

The separation of `fork()` and `exec()` allows the shell to do a whole bunch of useful things rather easily, for example:

```bash
wc main.c > newfile.txt
```

In this example, the output of the program `wc` is redirected into the output file `newfile.txt` the way the shell accomplishes this task is quite simple, when the child is created, before calling `exec` the shell closes the standard output and opens the file `newfile.txt` as the standard output, then when the `wc` program runs, it writes its output to the file `newfile.txt` rather than to the screen.

the following code shows a program that does exactly that:

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <unistd.h>

int main(int argc, char const *argv[]) {
    int rc = fork();
    if (rc < 0) {  // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) {  // child (new process), redirect standard output to the file
        close(STDOUT_FILENO);
        open("./output.txt", O_CREAT | O_WRONLY | O_TRUNC, S_IRWXU);

        // now exec "wc"...
        char *myargs[3];
        myargs[0] = strdup("wc");      // program: "wc" (word count)
        myargs[1] = strdup("main.c");  // argument: file to count
        myargs[2] = NULL;              // marks end of array
        execvp(myargs[0], myargs);     // runs word count
    } else {                           // parent goes down this path (main)
        int wc = wait(NULL);
    }
    return 0;
}
```

The reason this redirection works is due to an assumption about how the os manages file descriptors. Unix systems start looking for free file descriptors at zero, in this case, `STDOUT_FILENO` will be the first free file descriptor and thus get assigned when `open()` is called, subsequent writes by the child process process to the standard output file descriptor, (i.e. `printf`) will be written to the file `output.txt` instead of the screen.

When you run `main.c` and check the `output.txt` file, you will see the output of the `wc` program.

```txt
 28 117 926 main.c
```

Unix pipes are implemented in a similar way, but with the `pipe()` system call,l in this case, the output of one program is connected to an in-kernel pipe (queue), and the input of another program is connected to the same pipe, thus the output of the first program is sent directly to the input of the second program.

Long and and useful chains of commands can be strung together, as a simple example, consider the looking for a word in a file, and then counting how many times that word appears, with pipes and the utilities `grep` and `wc` it is easy:

```bash
$ grep "os" "Chapter 5 - Interlude: Process API.md"  | wc -l
10
```

## Other parts of the api

Beyond `fork()`, `exec()`, and `wait()`, there are a lot of other interfaces for interacting with processes in unix systems, for example the `kill()` system call which is used to send signals to a process to go to sleep, die and other useful imperatives. The entire signal subsystem provides a rich infrastructure to deliver external event to processes including ways to receive and process those signals.

There are many command line tools that are useful as well like the `ps` command which lists all the processes running on the system, the `top` command which shows the top processes running on the system and how much they are eating up, the `kill` command which sends signals to processes, and the `strace` command which traces the system calls made by a process.
