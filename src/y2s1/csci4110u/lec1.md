# Introduction

\##### TODO: Clean up this section

# Vegreville Pysanka

## Basics

image - mathematical concept, continuous (ex: a fn - f(x,y) :-> $R^3$)
+ approximate into pixel grid
+ computer can't use real numbers, floating-point == approximation

## Review of important concepts

## Mathematics Background

+ color rendering + global illumination <- integrate across surface of pixel/point(hemisphere)

\##### End TODO: Clean up this section

# Graphics Hardware - Part One

Gajillion foot view of interested computer hardware:
```mermaid
flowchart LR
  m[Memory] <--> c[CPU] <--> g["Graphics Card"] --> d[Display]
```

## Memory

+ CPUs work directly on data within registers.
+ Register work as quickly as the CPU. No delay in access.
+ Followed by L1 cache.
  - Near CPU speed. Little delay.
  - Feeds CPU registers.
+ Then L2 cache.
  - Slower, ~order of magnitude.
  - Feeds L1 cache.
  - Larger than L1 cache.
+ Some systems have an L3 cache.
  - Again, larger but slower.
+ Lastly, there is DRAM.
  - Very large storage.
  - Much slower.

## Cache

The following numbers are old and thus not super accurate:
+ L1 cache is typically 16Kb.
+ L2 cache: 256Kb.
+ L3 cache: Multiple megabytes.

### Cache Lines

```admonish danger
This information is important when optimizing code.
```

+ A cache line is the basic amount of data that will be transferred at a time.
+ 64 or 128 bytes of _contiguous_ memory.
+ Most data types are multiple bytes large.
  - We'll access these bytes together.
  - So they should fit in a single cache line.
  - If data is not "aligned" to a cache line boundary, we may have to transfer
    an additional cache line worth of bytes to load the data.
    * Consider a double (8 bytes) where the middle bytes cross a cache line
      boundary.

## DRAM

+ Large and cheap.
+ Optimized for size not speed.
+ A DRAM Cell stores 1 bit.
  - Stores the bit for 30 to 100 milliseconds.
  - Contents must be refreshed regularly.
+ Chips are typically organized as a 2D array of cells.
  - You retrieve a row at a time.
  - The retrieved row is stored in a fast row buffer.
    * First read of a row is slow, subsequent reads are faster.
    * Allows adjacent bits to retrieved quickly.
+ Reads are destructive.
  - Contents of row buffer must be written back to memory.
+ Designed to feed caches.
+ **Don't** jump all over DRAM.

So:
+ Avoid referencing DRAM if possible.
+ Reference contiguous locations.
+ Align data to cache lines.
+ Make data structures as compact as possible.

