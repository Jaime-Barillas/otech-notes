# Concurrency, Parallelism, and Scheduling

Consider a program made up of individual tasks:
+ **Concurrency** - Is when these tasks are **capable** of being
  executed at the same time.
+ **Parallelism** - Is when these tasks **are** executed at the
  same time.

In this sense, parallel programs are a subset of concurrent
programs. Where concurrent programs are capable of, but may decide
against, executing their tasks at the same time, parallel programs
certainly do.

+ A concurrent program that does not run its tasks at the same time
  may still progress in each task simultaneously.
  - For example, by interrupting progress in one task to work on another.

![Tasks](03-tasks.jpg)

+ **Time Slicing** - The way tasks are cut up and scheduled on the CPU.
  - Generally, the goal is to keep the CPU busy with tasks.

## Time Slicing

+ Often used to refer to a particular scheduling order.
+ Allows running more threads on fewer cores if needed.

ex: 3 tasks, 2 CPUs
+ A task might be split up into multiple "time slices".
  - Each time slice will be assigned to a CPU.
  - Each task may be assigned to different CPUs at different times.
+ Maximizes hardware use (maximum parallelism) even if the tasks
  themselves are not fully parallel.
+ Can also be considered as the problem of choosing of when CPUs
  swap tasks.
  - **Context Switches** - Swapping from one task (thread) to
    another. Also called **Interleaving Points**.
  - Happens often when the task awaits some result/resource
    (possibly shared).

## Processes and Threads Review

+ Process:
  - An instance of program execution.
  - The execution context of a running program.
    * i.e. the resources associated with a program's execution.
+ Threads:
  - Light weight processes.
  - Threads share process state among multiple threads.
    * Greatly reduces cost of switching context.

## A Parallel Program May Not Execute Tasks in Parallel

+ Not enough hardware (cores).
+ Inter-task dependencies forcing linear execution.
+ Job scheduling may schedule other programs on free cores instead.

## Making Parallel Programs

1. Start with a sequential program.
1. Divide the program into tasks.
   + Identify shared/local data.
1. Organize tasks into threads.
   + Consider shared/local data.
1. Write parallel versions of code guided by steps 2 and 3.

