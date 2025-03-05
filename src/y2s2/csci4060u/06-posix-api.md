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
+ `pthread_attr_init(attr)`
+ `pthread_attr_destroy(attr)`

trylock() into lock() slower than just lock() at
sufficiently large number of threads compared to
additional work. Thread/lock/resource **contention**.
