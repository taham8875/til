# Chapter 7 - Scheduling: Introduction

In this chapter, we will present a s series of scheduling policies that smart people have developed over the past years.

## Workload assumptions

Before getting into the range of possible policies, let's first make a number of simplifying assumptions about the processes running in a system (a workload). Determining the workload is a critical part of building policies.

The workload assumptions we make are unrealistic, but they are useful for understanding the basic issues in scheduling.

We will make the following assumptions about the processes, (also called jobs) that are running on the system:

1. Each job runs for the same amount of time.
2. All jobs arrives at the same time
3. All jobs only uses the cpu (no I/O)
4. The run-time of each job is known

## Scheduling metrics

Beyond making workload assumptions, we also need one more thing to enable us to compare different scheduling policies, (scheduling metrics) (To be able to measure)

For now let's start simple by having single metric (turnaround time) which is the time from when a job arrives until it completes.

$$ T_{turnaround} = T_{completion} - T_{arrival} $$

Because we have assumed that all jobs arrive at the same time, therefore $T_{arrival}$ is the same for all jobs, and we can ignore it. Hence $T_{turnaround} = T_{completion}$

Notice that turnaround time is a performance metric, which will be our primary focus this chapter. Another metric is fairness, which means that all jobs should be treated fairly. Performance and fairness are often in conflict, a scheduler for example may optimize performance but at the cost of preventing some jobs from running.

## First in, First out (FIFO)

The most basic policy is the first in, first out (FIFO) policy. In this policy, it has number of positive properties, it is clearly very simple and easy to implement.

**Conovy effect**: The main problem with FIFO is that it can lead to the convoy effect, where a long job holds up shorter jobs behind it. In other words, a number of relatively short potential consumers of a resource get queued behind a heavy resource consumer. This is because the first job to arrive runs to completion, and the next job starts only after the first job completes.

## Shortest Job First (SJF)

An easy algorithm to implement is the shortest job first (SJF) algorithm. In this algorithm, the scheduler selects the waiting job with the smallest estimated run-time to execute next (the next job to run).

In fact, given the assumption that all jobs arrive in the same time, we could prove the SJF is indeed the optimal solution, however in if they didn't arrive in the same time, it will be more probable to have the shorter jobs comes when longer ones processing, causing conovy problem again.

## Shortest Time-to-Completion First (STCF)

There is a scheduler which does exactly that: add preemption to sjf, known as the shortest time-to-completion first (STCF) algorithm. In this algorithm, Any time a new hob enters the system it determines of all the jobs in the system which has the least time left, and then schedules that one to run next.

Thus, if we knew that job lengths and jobs only uses the cpu and our only metric what turnaround time, then STCF would be the optimal policy.

In newer computer systems, a new metric was born, response time. Response time is defined as the time from when a job arrives until the first response is produced:

$$ T_{response} = T_{first run} - T_{arrival} $$

STCF is not optimal for response time and interactivity, because it waits for smaller jobs to finish before starting larger ones. Imagine sitting at your terminal, waiting for 10 seconds to see a response from the system because some other job is scheduled to run first.

## Round Robin (RR)

The basic idea of robin hood is simple, instead of running jobs to completion, RR runs a job for a time slice (scheduling quantum) and then switches to the next job. It repeatedly does this until all jobs are done.

For that, RR is sometimes called time slicing, Note that the length of the time slice must be multiple of the time interrupt period; thus if the timer interrupts every 10 milliseconds, the time slice could be 10, 20, 30, and so on.

This time slice length is crucial for RR, the shorter it is, the better the performance of the RR under the response time metric, but making it too short can be a problem as the cost of context switching will be dominant over the performance, thus deciding the time slice presents a trade off to a system designer make it a long enough to not waste time on context switching, but short enough to provide good response time.

RR with a reasonable time slice is good policy if the response time is only our metric, but what about turnaround time, it is not optimal for that metric, because it tries to stretch the time of each job to the maximum, thus the turnaround time will be longer than the optimal. Even worse than FIFO in many cases.

More generally:

- Any policy that is fair, will perform poorly on turnaround time
- Any policy that is unfair, will perform poorly on response time

## Incorporating I/O

We will relax our third assumption, all the programs perform io, a scheduler clearly has a decision to make when a job initiates an io request, as the current running job will not be using the cpu, it will be blocked for a while waiting for the io completion.

A scheduler has to make a decision when the io completes, and when that occurs, an interrupt is raised and the os runs and moves the process the issued the io from blocked to ready state.

A common approach is to treat each 10 ms sub job of A as an independent job. Doing so allows for overlapping of io and cpu time, and thus can improve the performance of the system.
