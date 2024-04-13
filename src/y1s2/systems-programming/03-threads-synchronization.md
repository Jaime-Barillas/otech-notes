# Threads

A thread is the basic unit of CPU utilization. Thread creation is lightweight
compared to process creation. Threads have their Parents’ PCB, its own Thread
Control Block, register set, Stack and common Address space

Threads contain:
+ ID
+ Program Counter
+ Register Set
+ Stack

# Multicore Programming

Parallelism, concurrency. Tasks for developers include:
+ Identifying parallelizable/concurrent tasks.
+ Balancing parallelism/concurrency.
+ Data splitting.
+ Data dependencies.
+ Testing, debugging.

# Threads

User level thread libraries and kernel level threads exist.
We use `pthread` in this course.

pthreads can be implemented at the user level or kernel level.
pthreads is primarily a standard. OS's may implement this standard.
+ `#include <pthread.h>`
+ `pthread_create()`
+ `pthread_join()`
+ `pthread_self()`

---

# Producer, Consumer Pattern

self explanatory.

## Problems

Producer may produce more than the buffer can hold. (possible buffer overflow).
Consumer may attempt to retrieve data after the buffer is emptied. This is
known as the "Bounded Buffer Problem".

The solution: ensure both the consumer + producer know the capacity and current
size of the buffer. The producer will wait to produce if needed, the consumer
will wait to retrieve if the buffer is empty.

## Race Conditions

Non-deterministic order of execution of concurrent/parallel code with shared
mutable state.

Or

Race Conditions ocurr when two or more threads/processes can access shared data
and attempt to access and change that data at the same time. The currently
active thread/process can change at any time in between executions of
individual instructions potentially causing the threads/processes to access the
shared data in an arbitrary order. The typical solution is to place a lock
around the shared data.

## Critical Sections

Sections of code where shared mutable data is either read or written to.

Can be broken down into the following subsections:
+ entry - The thread/process checks if it is okay to enter its critical section.
+ critical - The dangerous area.
+ exit
+ remainder

The entry and critical sections are the most important.

## Supporting Critical Sections

### Mutual Exclusions

Only one process should execute code in their critical section at a time.

#### Mutexes

Mutexes facilitate this: first `acquire()` a lock, later `release()` the lock.
These two functions need to be atomic and may require hardware support. Trying
to acquire an already acquired lock will cause "busy" waiting. this kind of
behaviour is also called a _spinlock_.

### Progress Guaranteed

If no process is in its critical section, and a process wants to enter its
critical section, _selection_ of the next process to enter its critical section
cannot be postponed indefinitely.

### Bounded Waiting

There is a bound on the number of process's that can cut in line when multiple
are waiting to enter their critical sections.

### Semaphores

Binary Semaphore - Same as Mutex.
Counting Semaphore - internal counter can range over an unrestricted domain.
Use atomic 'wait' and 'signal' functions.
