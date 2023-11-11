# Video - Concurrency in Go

[link](https://youtu.be/LvgVSSpwND8)

One note is worth mentioning is that concurrency != parallelism.

Concurrency is about executing multiple tasks in overlapping time periods. allowing dealing with multiple tasks even if the tasks don't run simultaneously.

Parallelism is about executing multiple tasks simultaneously typically using multiple processors or cores, thus enhancing the performance.

Concurrency can be achieved in single core machine via switching, while parallelism requires multiple cores.

# Simple Synchronous Program

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	count("sheep")
	count("fish")
}

func count(thing string) {
	for i := 1; true; i++ {
		fmt.Println(i, thing)
		time.Sleep(time.Millisecond * 500)
	}
}
```

This program is synchronous, which means that the program will wait for the first function to finish before executing the second function. So it will print `sheep` infinitely until the program is terminated.

To make this program asynchronous, we can use goroutine, just by adding `go` keyword in front of the function call.

```go
func main() {
	go count("sheep")
	count("fish")
}
```

Now it will print `fish` and `sheep` alternatively.

Now if you just want to count the sheeps only and removed the `count("fish")` call, the program will terminate immediately. This is because the main function is terminated, and the program will exit.

To solve this problem, we can use `sync.WaitGroup` to wait for all goroutines to finish.

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var wg sync.WaitGroup
    wg.Add(1)
    go func() {
        count("sheep")
        wg.Done()
    }()
    wg.Wait()
}

func count(thing string) {
    for i := 1; true; i++ {
        fmt.Println(i, thing)
        time.Sleep(time.Millisecond * 500)
    }
}
```

Golang `Waitgroup` allows you to block a specific code block to allow a set of goroutines to complete execution

- `wg.Add(1)` adds 1 to the waitgroup counter, if there are more than 1 goroutines, you can add more.
- `wg.Done()` decrements the counter by 1, which is the same as `wg.Add(-1)`
- `wg.Wait()` blocks the execution of the program until the counter is 0.

# Channels

Channels are used to communicate between goroutines. It is a typed conduit through which you can send and receive values with the channel operator, `<-`.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	c := make(chan string)
	go count("sheep", c)

	for {
		msg, open := <-c
		if !open {
			break
		}
		fmt.Println(msg)
	}
}

func count(thing string, c chan string) {
	for i := 1; i <= 5; i++ {
		fmt.Println(i, thing)
		time.Sleep(time.Millisecond * 500)
	}
	close(c)
}
```

Let's break down the code above.

- `c := make(chan string)` creates a channel that can only send and receive string values.
- `go count("sheep", c)` starts a goroutine that calls the `count` function with the channel `c` as the second argument.
- `msg, open := <-c` receives a message from the channel `c`. If the channel is closed, `open` will be false.
- `close(c)` closes the channel `c`. If you don't close the channel, the program will panic after all the messages are received.

Note you should close the channel at the sender but never at the receiver, because the sender is the one who knows when the channel should be closed and the receiver don't know when the channel will be closed.

A shorter way to write the code above is to use `range` to iterate over the channel.

```go
	for msg := range c {
		fmt.Println(msg)
	}
```

# Buffered Channels

By default, channels are unbuffered, which means that they will only accept sends (`chan <-`) if there is a corresponding receive (`<- chan`) ready to receive the sent value. Buffered channels accept a limited number of values without a corresponding receiver for those values.

```go
// This will not work
func main() {
    c := make(chan string)
    c <- "hello"
    fmt.Println(<-c)
}
```

To make the channel buffered, we can specify the buffer length in the second argument of `make` function.

You can buffer it with one value.

```go
func main() {
    c := make(chan string, 1)
    c <- "hello"
    fmt.Println(<-c)
}
```

or with more values.

```go
func main() {
    c := make(chan string, 2)
    c <- "hello"
    c <- "world"
    fmt.Println(<-c)
    fmt.Println(<-c)
    // if we add one more senders or receivers, the program will panic
    // c <- "Hi" // panic
    // fmt.Println(<-c) // panic
}
```

# Select

The `select` statement lets a goroutine wait on multiple communication operations. So it will block until one of the send/receive operation is ready. If multiple operations are ready, it will randomly choose one to execute.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        for {
            c1 <- "Every 500ms"
            time.Sleep(time.Millisecond * 500)
        }
    }()

    go func() {
        for {
            c2 <- "Every 2 seconds"
            time.Sleep(time.Second * 2)
        }
    }()

    for {
        select {
        case msg1 := <-c1:
            fmt.Println(msg1)
        case msg2 := <-c2:
            fmt.Println(msg2)
        }
    }
}
```

This will print `Every 500ms` and `Every 2 seconds` alternatively.

If we naively did this

```go
for {
    msg1 := <-c1
    msg2 := <-c2
    fmt.Println(msg1)
    fmt.Println(msg2)
}
```

The program will print `Every 500ms` and `Every 2 seconds` every two seconds, because the program blocks until both channels are ready.

# Worker Pools

A worker pool is a collection of goroutines that concurrently perform the same task. Basically, it is a queue of tasks that are waiting to be executed by the goroutines (workers). This approach optimizes resource utilization and manages concurrent task execution efficiently. Workers continuously listen for and execute tasks from the pool.

For example, we have a program that calculates fibonacci numbers.

```go
package main

import (
    "fmt"
)

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    go worker(jobs, results)
    go worker(jobs, results)
    go worker(jobs, results)
    go worker(jobs, results)

    for i := 0; i < 100; i++ {
        jobs <- i
    }
    close(jobs)

    for j := 0; j < 100; j++ {
        fmt.Println(<-results)
    }

}

func worker(jobs <-chan int, results chan<- int) { // this reduces the possibility of misuse by detecting the direction of the channel
    for n := range jobs {
        results <- fib(n)
    }
}

func fib(n int) int {
    if n <= 1 {
        return n
    }
    return fib(n-1) + fib(n-2)
}
```

Let's break down the code above.

- `jobs := make(chan int, 100)` creates a channel that can send and receive int values with a buffer length of 100.
- `results := make(chan int, 100)` creates a channel that can send and receive int values with a buffer length of 100.
- `go worker(jobs, results)` starts a goroutine that calls the `worker` function with the channel `jobs` and `results` as the arguments.

The more workers you have, the faster the program will finish and more resources will be used.

- `jobs <- i` sends the value `i` to the channel `jobs`.
- `close(jobs)` closes the channel `jobs`. If you don't close the channel, the program will panic after all the messages are received.
- `fmt.Println(<-results)` receives a message from the channel `results`.
- In the worker function, for each value received from the channel `jobs`, it will send the result of `fib(n)` to the channel `results`.
- `fib(n)` calculates the fibonacci number of `n`.
