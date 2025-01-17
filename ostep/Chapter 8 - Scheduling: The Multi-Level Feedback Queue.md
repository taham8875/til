# Chapter 8 - Scheduling: The Multi-Level Feedback Queue

In this chapter we will tackle the problem of developing one of the most well known approaches to scheduling, Multi level feedback queue (MLFQ).

The fundamental problem MLFQ tries to address:

- We want to optimize turnaround time, which is done by running shorter jobs first. Unfortunately the os does not know the length of the job, the exact requirement by SJF or STCF.
- We want to minimize the response time, so the system feels interactive, (RR minimizes response time, but not turnaround time).

In summary we want to design a scheduler that both minimizing the response tim for interactive jobs and also minimizing turnaround time without prior knowledge of the job length.

## MLFQ Basic Rules

MLFQ has a number of distinct queue, each assigned a different priority level, at any given time, a job that is ready to run is on a single queue, MLFQ uses priority to decide which job to run next.

If more than one job on a given queue have the same priority, then the scheduler uses RR among them.

Thus, the key in MLFQ is how it sets t he priorities, MLFQ sets the priority of jobs based on it observed behavior, if, for example, a job gives up the cpu repeatedly waiting for an input form the keyboard, then it should be moved to a higher priority queue as it is an interactive process. Or if a process uses the cpu extensively, then it should be moved to a lower priority queue.

Thus MLFQ learns about processes as they run, and uses the history of the process to make decisions about the future.

In summary:

- Rule 1: If priority(A) > priority(B), A runs (B doesn't).
- Rule 2: If priority(A) = priority(B), A & B run in RR.

Of course, if a job C has a lower priority than A or B, then C will not run until A and B are done. Which may cause the poor C job not to run for a long time, an outage!

Static priority does not help at all, therefore, we will discuss how job priorities are changing over time.

## Attempt 1: How to Change Priority

Let's keep in mind our workload:

- A mix of interactive jobs that are short running (and may frequently gives up the cpu)
- Some longer running jobs that needs a lot of cpu time where the response time is not important.

Here is our first attempt at a priority changing algorithm:

- Rule 3: When a job enters the system, it is placed at the highest priority (the topmost queue).
- Rule 4: If a job uses up an entire time slice while running, its priority is reduced (it moves down one queue).
- Rule 5: If a job gives up the cpu before the time slice is up, it stays at the same priority level.

Because it does not know whether a job will be an short job or a long running job, it first assumes that it might be a short job, it will run quickly and complete, if it is not a short job, it will slowly move down the queue.

### What about the io?

If the process performs a lot of io so that if releases the cpu before the time slice is up, it will stay at the same priority level, we don't want to punish this process as it is interactive and repeatedly gives up the cpu. MLFQ is designed to help interactive jobs.

## Problems with Attempt1

Starvation, if there are many interactive jobs in the system, then the long running jobs will never run, as they will always be preempted by the interactive jobs.

Also, a hacky use may game the scheduler algorithm, by releasing the cpu just before the time slice is up, (just use the 99% of the time slice then issue an io operation to a file you don't really care about), then this job will monopolize the cpu.

## Attempt 2: The Priority Boost

To solve the starvation problem, we can add a rule to the scheduler to guarantee the cpu bound jobs will make some progress (even if it is not so much)

- Rule 5: After some time period S, move all the jobs in the system to the topmost queue.

Our new rule solves two problems at once:

- First, processes are guaranteed not to starve, by sitting in the top queue, a job will share the time with other high priority jobs and eventually receive service.
- Second, if at some point the cpu pound process became interactive, it the scheduler deals with it properly once it had the priority boost.

## Attempt 3: Better Accounting

how to prevent gaming of the scheduler?

The solution is performing better accounting of the cpu time at each level of the MLFQ. Instead of forgetting how much of a time slice a process used at a given level, the scheduler should keep track, once a process has used its allotment, it is moved down a level. So whether it uses the time slice in long burst or many smaller ones does not matter.

So we rewrite rule 4 as:

- Rule 4: Once a job uses up its time allotment at a given level (regrading of how many times it has given up the cpu), its priority is reduced.

## Tuning MLFQ And Other Issues

One question is how to parameterize such a scheduler. For example, how many queues should there be? How long should the time slice be? How long should the time for boosting be? There are no easy answers to these questions, only some experience with the workload and tuning the values based on that experience.

For example, most MLFQ variants allow for varying the time slice based on the priority level, with the higher priority levels getting shorter time slices.

Other MLFQ variants waiting for an administrator to tweak the parameters tables based on the workload.

Others don't use tables but uses mathematical expressions to calculate the time slice based on the priority level. (For example FreeBSD)

Some scheduling reserves the highest priority queue for operating system works, thus typical users can never obtain the highest levels of the priority queue.

## Summary

MLFQ is a powerful scheduling algorithm that can adapt to the workload, it is designed to optimize turnaround time and response time, and it does so by changing the priority of jobs based on their observed behavior.

These are the rules of MLFQ:

- Rule 1: If priority(A) > priority(B), A runs (B doesn't).
- Rule 2: If priority(A) = priority(B), A & B run in RR.
- Rule 3: When a job enters the system, it is placed at the highest priority (the topmost queue).
- Rule 4: Once a job uses up its time allotment at a given level (regrading of how many times it has given up the cpu), its priority is reduced.
- Rule 5: After some time period S, move all the jobs in the system to the topmost queue.
