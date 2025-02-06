# Multicore Hardware

The goal: Increase performance.

**Speedup** $ = \frac{t_s (\text{Execution time on a single processor})}{t_p (\text{Execution time with } p \text{ processors})}$

+ **Linear Speedup** - A speedup factor of $p$ with $p$ processors.
+ **SuperLinear Speedup** - A speedup factor greater than $p$.
  - Is this possible?
  - The parallel parts of the program can must execute faster than
    $t_s$.
  - Possible when parallelization presents opportunities for
    optimizations.
    * Multicore system processors may have more memory than single
      processor systems.
    * Hardware accelerators may be used in multiprocessor systems
      that are not available for single processor systems.
    * Nondeterministic algorithms. e.g. a solution can be found
      quickly in one part of the parallel implementation.

## Parallel Architectures

Grouped by Instruction stream and Data stream.
![Architecture Matrix](02-sisd-simd-matrix.png)

+ **SISD** - Single Instruction Single Data.
  - Single CPU.
  - Instruction level parallelism.
+ **SIMD** - Single Instruction Multiple Data.
  - Many core processors.
  - **Data Level Parallelism** - Apply a single instruction to
    multiple instances of data at the same time.
+ **MISD** - Multiple Instruction Single Data.
  - Each processor performs different instructions on the same data.
  - Uncommon as an architecture, can be simulated by MIMD.
+ **MIMD** - Multiple Instruction Multiple Data.
  - **Thread Level Parallelism** - Each processor has its own set of
    instructions and possibly data.

### SIMD vs. MIMD

SIMD:
+ Single instruction stream, multiple data streams.
+ Data level parallelism can be exploited.

MIMD:
+ Multiple instruction streams, multiple data streams.
+ Thread level parallelism can be exploited.
+ Relatively low cost to build due to the use of the same processors
  as those found in single processor machines.
+ More flexible than SIMD.
  - Can simulate other architectures.
+ Two main sub-categories of MIMD:
  - **Centralized Shared-Memory**.
    * $< 100$ processors.
    * Memory can become a bottleneck, limiting core count.
  - **Distributed-Memory**.
    * $> 100$ processors.
    * Communication becomes difficult (e.g. syncing, stale data).

### Centralized Shared-Memory Architectures

+ Multiple processors on the same die.
+ All processors share memory and I/O devices.

+ **Symmetric Multiprocessor (SMP)**
  - Shared address space.
  - "Equal-time" access for each processor.
  - OS treats every processor the same way.
+ **Non Uniform Address Space Multiprocessor (NUMA)**
  - Different memory regions have differnt access costs.
    * E.g. "Near" vs "Far" memory.

### Distributed-Memory Architectures

+ Two important aspects:
  1. Processors - Often not located on the same chip.
     - ex: Clusters.
  2. Interconnect Network.
     - High speed interconnection network for communication across
       processors.
     - Even across long distances (e.g. typically fibre-optics).
+ Can have a shared memory address space or multiple address spaces.
  - If using shared memory address spaces: communication can be done via _load_
    and _store_ instructions.
  - If using multiple address spaces: communication is done via
    **message-passing**.
    * **MPI** - See the message passing library for C.
+ Using shared data can have speed consequences if loading/storing
  across the interconnect network.
  - But the data is kept up-to-date for other processors.
+ Using message passing, cores work on local data then send results
  to others. Has potential for data synchronization issues.

### Taking Advantage of MIMD

+ Run multiple processes at the same time.
+ Build a program that runs multiple threads at the same time.

### Instruction-Level vs. Thread-Level Parallelism

+ Hardware improvements can have an affect on how we develop software.
+ _Instruction level_ parallelism is typically independent of whether
  software is sequential or concurrent.
+ Thread level parallelism (like multicore programming) usually _is_
  dependent on the software being concurrent.
+ **Instruction Level Parallelism** - The many instructions contained in each
  thread. (Low Level, internal to CPU.)
+ **Thread Level Parallelism** - Multiple threads per program. (High Level.)
+ **Multithreading** - An _instruction_-level approach to multi-threaded
  programs.
  - Run multiple threads _simultaneously_.
  - Can be used on a single processor system.
  - Switch between threads using:
    * **Fine-Grained Multithreading** - Potentially between every
      instruction.
    * **Coarse-Grained Multithreading** - During an expensive stall.
  - Each thread has its own program counter and memory (among other
    things.)
  - Intel **Hyperthreading** is one approach using _Simultaneous
    Multithreading_ (SMT).

### Symmetric Multicore Design and Asymmetric Multicore Design

+ Historically, we started with multiple physically separate CPUs in
  one system.
+ Then multiple cores on the same chip with an _internal_ bus
  connecting the various cores.
+ To, most recently, **mixed-core processors** or an **Asymmetric
  Multicore Design**, Different _types_ of cores on the same chip.

### Massively Parallel Systems

+ **GPU Computing** (GPGPU)
  - 100s or more GPUs.
+ **Massively Parallel Processor Arrays (MPPAs)**
  - Array of 100s of CPUs + RAM.
+ **Grid Computing**
  - Nodes performing different tasks.
+ **Cluster Computing**
  - Nodes performing the same task.

