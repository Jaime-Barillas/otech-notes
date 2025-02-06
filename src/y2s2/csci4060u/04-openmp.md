# OpenMP

## Explicit vs Implicit

**implicit** - Do _not_ explicitly write parallel code. e.g. Use of
annotations, such as compiler directives, to generate a parallel version of
your code.
**explicit** - Explicit use of threads and organizing code into parallelizable
chunks.

# OpenMP

+ Uses compiler directives.
  ```
  #pragma omp <construct> [clause ...]
  ```
+ Uses the `omp.h` header.
+ Most OpenMP constructs apply to a structured block:
  ```
  #pragma omp parallel
  {
    //Code here...
  }
  ```
  - Can have `exit()` at end of structured block.

## Constructs of Note

+ `#pragma omp parallel` - Denote a parallel region.
  - `#pragma omp parallel num_threads(n)` - Parallel region with n threads.
  - Variables declared _outside_ the pragma block are shared amongst threads.
  - Only $n - 1$ threads are created. The last parallel section is run on the
    control thread.

## Functions of Note

+ `omp_set_num_threads()` - Set number of threads.
+ `omp_get_thread_num()` - Get thread id.

## Env Vars

+ **OMP_NUM_THREADS** - Set the number of threads used in parallel code.

## Fork-Join

+ The Main/Control thread spawns a team of child/spawned threads as needed.
+ implicit barrier (by openmp) at join point.

## Threads Communicate!

+ By sharing data.
+ **Race Condition** - Unintended sharing of data.
  - Race conditions ocurr when the program's outcome changes as the threads
    are scheduled differently.
+ Use synchronization techniques to protect data conflicts and prevent race
  conditions.
+ Change how data is accessed to minimize the need for expensive
  synchronization.

## Performance Pit-Falls

+ **False Sharing** - If independent data elements (accessed by different
  threads) happen to sit on the same cache line, each update will need to be
  reflected across cpu caches.
  - If this happens with array elements, contiguous elements may share cache
    lines. Pad arrays so independent elements are on distinct cache lines.
    * Cache lines are typically 64bytes long.

## Synchronization

+ Bringing one or more threads to a well defined and known point in their
  execution.
+ Used to impose order constraints and protect access to shared data.
+ Some common forms:
  - **Barrier** - Each thread waits at the barrier until _all_ threads arrive.
  - **Mutual Exclusion** - A block of code that only one thread may execute at
    a time.
+ OpenMP High-level synchronization:
  - Critical.
  - Atomic.
  - Barrier.
  - Ordered.

### Barriers

Each thread waits until all threads reach the barrier.
```
#pragma omp barrier
```

+ explicit vs implicit.
+ openmp has some implicit ones.
  - e.g. at end of parallel block.

### Critical

+ Mutual Exclusion.
+ Only one thread at a time can enter a critical region.
+ Can be used to remove the impact of false sharing.
```
#pragma omp critical
```

### Atomic Constructs

+ Provides mutual exclusion, but only to the read/write/update/capture of a
  memory location.
+ An atomic construct ensures that a specific storage location is accessed
  atomically.
+ Can be used to remove the impact of false sharing.
```
#pragma omp atomic <read|write|update|capture>
```

## SPMD - Single Program Multiple Data And Worksharing

+ A program where each thread executes the same code.
+ **Worksharing** - Splitting up pathways through the code between threads.
  - There are different ways to achieve this:
  - Loop constructs.
  - Section(s) constructs.
  - Single construct.
  - Task construct.

### The Loop Construct

+ Splits up loop iterations amongst threads.
+ How loop iterations are mapped onto threads can be defined with the
  `schedule` clause (See: OpenMP Slides/docs).
+ Basic approach:
  - Find compute intensive loops.
  - Make loop iterations independent.
  - Place OpenMP directives
  - Test.
+ Can handle nested loops.
  - Transforms into a single loop of $N \times M$ size. This is parallelized.
```
#pragma omp for
#pragma omp parallel for
#pragma omp parallel for collapse(<num-including-nested>)
```

## Reductions

+ How do we handle cases like this:
```
// A reduction!
double accu = 0.0
double A[MAX];
for (int i = 0; i < MAX; i++) {
  accu + = A[i];
}
accu = accu / MAX;
```
+ There is a dependence between loop iterations that can't be trivially
  removed.
+ Use a reduction clause!
```
#pragma omp parallel for reduction (<op>:<list-of-shared-vars>)
```

## Running Sequentially

+ Guard the `omp.h` include with an `#ifdef _OPENMP`.
+ Add `#define`s for each openmp function.

---

lab 01, why is parallel slower.
+ Accessing arrays in parallel and _cache_ issues.
  - Cross core caches must be kept in sync.
    * "False sharing" - false because not needed technically.
    * Pad so that different array elements are on _different cache lines_.

