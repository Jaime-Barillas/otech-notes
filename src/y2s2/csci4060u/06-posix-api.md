# Posix API

There are four main groups of interest:
+ Thread Management
  - Creating threads.
  - Detaching threads.
  - Joining threads.
  - Get/set thread attributes.
  - etc.
+ Mutexes
  - Creating/Destroying Mutexes.
  - Locking/Unlocking Mutexes.
  - Get/set Mutex attributes.
+ Condition Variables
  - Communication between threads that share a mutex based on programmer
    specified conditions.
  - Create/Destroy condition variables.
  - Wait based on value of a condition variable.
  - Signal based on value of a condition variable.
  - Get/set condition variable attributes.
+ Synchronization
  - Functions that manage read/write locks and barriers.

## Thread Functions

+ `pthread_create(thread_id, attr, fn, args)`
+ `pthread_exit(status)`
+ `pthread_cancel(thread_id)`
+ `pthread_attr_t`
+ `pthread_attr_init(attr)`
+ `pthread_attr_destroy(attr)`
+ `pthread_attr_setdetachstate()`
+ `pthread_detach()`
+ `pthread_attr_getstacksize(attr, stacksize)`
+ `pthread_attr_setstacksize(attr, stacksize)`
+ `pthread_attr_getstackaddr(attr, stackaddr)`
+ `pthread_attr_setstackaddr(attr, stackaddr)`
+ `pthread_self()`
+ `pthread_equal(thread1, thread2)`
+ `pthread_once(once_control, init_routine)`
+ `pthread_mutex_t`
+ `pthread_mutex_init(mutex, attr)`
+ `pthread_mutex_destroy(mutex, attr)`
+ `pthread_mutexattr_t`
+ `pthread_mutexattr_init(attr)`
+ `pthread_mutexattr_destroy(attr)`
+ `pthread_mutex_lock(mutex)`
+ `pthread_mutex_trylock(mutex)`
+ `pthread_mutex_unlock(mutex)`
+ `pthread_cond_t`
+ `pthread_cond_init(cond, attr)`
+ `pthread_cond_destroy(cond)`
+ `pthread_condattr_t`
+ `pthread_condattr_init(attr)`
+ `pthread_condattr_destroy(attr)`
+ `pthread_cond_wait(cond, mutex)`
+ `pthread_cond_signal(cond)`
+ `pthread_cond_broadcast(cond)`

Attributes include:
+ Detached/Joinable state.
+ Scheduling Inheritance.
+ Scheduling Policy.
+ Scheduling Parameters.
+ Scheduling contention scope.
+ Stack size.
+ Stack address.
+ Stack guard (overflow) size.

## Thread Binding and Scheduling

+ It is generally up to the imlementation or OS to decide where and when
  threads execute.
+ pthreads does provide several functions to specify how threads are scheduled
  for execution (e.g. FIFO, Round-Robin, Other).
+ pthreads does not provide functions for binding threads to specific cores,
  but non-standard (or OS specific) extensions may exist.

## Thread Termination

+ The thread returns normally from its starting function.
+ The thread calls `pthread_exit()`.
+ The thread is canceled by another thread via `pthread_cancel()`.
+ The entire process is terminated due to `exec()` or `exit()`.
+ `main()` terminates first without calling `pthread_exit()` itself.
  - Calling `pthread_exit()` will cause `main` to block and wait for the
    threads it created to finish.

## Mutexes

As used in pthreads:
+ Only one thread can lock (or own) a mutex at any given time.
+ For other threads to lock an owned mutex, the owning thread must unlock the
  mutex.
+ Can be used to prevent race conditions.
+ It is the programmer's responsibility to ensure that every thread that should
  use a mutex actually uses one.
+ Implement synchronization by controlling thread access to data.
+ There are three attributes defined by the standard:
  - Protocol - Specify the protocol used to prevent _priority inversions_.
  - Prioceiling - The priority ceiling of a mutex.
  - Process-Shared - Whether the mutex can be seen by threads in another
    process.
+ When more than one thread is waiting for a previously locked mutex that was
  just unlocked, the next thread to aquire the lock is dependent on the system
  scheduler (unless thread proiority scheduling is used).
+ `pthread_mutex_lock()` waits on a mutex.
  - Slower if there is additional non-multithreaded work to be done (has to
    wait for the thread to unblock).
+ `pthread_mutex_trylock()` does not block.
  - More resource intensive when polling a mutex.
+ `trylock()` into `lock()`.
  - If any additional non-multithreaded work can be done after the first
    `trylock()`, some time can be saved compared to `lock()`.
  - More overhead than just `lock()` so can be slower when many threads are
    used unless their is sufficient additional work to balance out the
    overhead.

trylock() into lock() slower than just lock() at
sufficiently large number of threads compared to
additional work. Thread/lock/resource **contention**.

## Condition Variables

+ Allow threads to synchronize based on the value of data.
+ Without conditions, threads would have to continuously poll to check if the
  condition is met.
  - Can be resource consuming.
+ Always used in conjunction with a mutex lock.
+ Use (roughly) this way:
  1. Declare global data which requires synchronization.
  2. Initialize a condition variable object.
  3. Initialize an associated mutex.
  4. Create two threads.
  5. In thread A:
     + Lock the mutex.
     + Check the value of the global variable.
     + If the condition hasn't been met, call `pthread_cond_wait()`.
       - This will unlock the mutex and wait until the condition object has
         been signalled.
     + When signalled, the thread resumes and the mutex is locked again.
     + Continue work, then unlock the mutex.
  6. In thread B:
     + Lock the associated mutex.
     + Change the value of the global variable that thread A is waiting on.
     + If the new value fulfills the condition, signal via the condition
       variable object.
     + Unlock the mutex.
+ One attribute exists for condition variables: `process-shared`.
  - Allows the condition variable to be seen by threads in other processes.

