# Chapter 9 - Scheduling: Proportional Share

In this chapter we will see proportional share scheduler (aka fair share scheduler). Proportional share is based around a simple concept: instead of optimizing for turnaround time or response time, we guarantee that each job obtain a certain percentage of the cpu time.

An excellent example of proportional share is the lottery scheduling, the basic idea is simple, each job is given a number of lottery tickets, and the scheduler draws a ticket to determine the next job to run. The more tickets a job has, the more likely it is to be selected. And processes that should run more often are given more tickets.

## Basic Concept: Tickets Represents Your Share

Tickets represent the share of a resource that a process should receive.

Lottery scheduling achieves the scheduling probabilistically but not deterministically. Which means that a process with more tickets will run more often, but it is not guaranteed to run in a certain order. or in the exact number of times. (If process A has 10 tickets and process B has 5 tickets, then A will run twice as often as B, but not exactly twice, may be 3 times or 1 time). But the longer the system runs, the more likely the number of times a process runs will be close to the expected value.

## Ticket Mechanisms

Lottery scheduling also provides a number of mechanisms to manipulate the tickets, one mechanism is **ticket currency**. Currency allows a user with a set of tickets to allocate tickets among their jobs in whatever currency would like. For example, a user with 100 tickets could allocate 10 tickets to the A job and 90 the B job, another user with 1000 tickets could allocate 500 to the A job and 500 to the B job. Although the user 2 gave more tickets to the B job than the user A did, but the weight of the tickets is not the same, user 1 gave the B job more share of the total tickets 90% than user 2 did 50%.

Another Helpful mechanism is **ticket transfer**, which allows a process to give tickets to another process. This is useful when a process is waiting for another process to complete, it can give its tickets to the other process, so that the other process can run more often. For example, a client/server setting, where a client process give sends a message to a server asking it to do something, to speed up the work, the client can pass the tickets to the server and thus maximize the performance of the server.

Finally, the **ticket inflation**, with inflation, a process can temporarily raise or lower the number of tickets it owns, in a competitive scenario with processes that do not trust one another, this make sense, one greedy process could give itself a big number of tickets and take over the machine, or in a trust environment where each processes trust each other, if one processes knew that it needs more cpu time, it can boost its tickets to get more cpu time. All without communication with any other process.

## Implementation

The implementation of lottery scheduling is simple, you need a random number generator and a data structure to track the processes of the systems (a list) and the total number of tickets.

To make it more efficient, it might generally be best to organize the list if sorted order from the hightest number of tickets to the lowest. The ordering does not affect the correctness of the algorithm, but it ensure the least number of iteration are made as the process with the most tickets will be selected first.

Note that unfairness is high at the beginning of the system, but as the system runs, the unfairness will decrease, and the system will become more fair.

## How to Assign Tickets

One problem we have not addressed with lottery scheduling is how to assign tickets to processes. Oen approach is to assume that the user knows best and each user is handed some number of tickets and can allocate tickets to any jobs they desire.

## Why Not Deterministic?

You might also wonder why we don't just use a deterministic policy, randomness gets us simple and approximately correct but it occasionally will not deliver the exact right proportions especially over short time scales, for that reason, **Stride Scheduling** is invented as a fair share scheduler that is deterministic.

Stride scheduling is also straightforward to implement, every system has a stride, which is inverse in proportional to the number of tickets a process has. We call the stride of each process and every time a process runs it will increment the counter for it to track the progress value.

The basic idea is simple, the next time the system runs a process, it selects the one with the lowest pass (stride) value so far.

Stride scheduling gets exactly right at the end of each scheduling cycle.

So after seeing stride scheduling you may ask, why lottery scheduling at all, well, lottery scheduling has one nice property that stride scheduling does not have, no global state, imagine a new job has entered in the middle of our stride scheduling. what should the pass value be, assigning it at zero make it monopolize the cpu, in lottery scheduling, there are no global state, we just add a new process with whatever tickets it has, and let it run. Lottery scheduling is much easier to incorporate new processes into the system in a sensible way.
