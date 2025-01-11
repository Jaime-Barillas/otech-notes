# Introduction

What is Computational Science?
+ Approximate solutions of mathematical problems in the (applied) sciences.

_A bunch of examples..._

## Exact vs Approximate

+ **Exact** - There is **zero** uncentainty.
  - $x = \ln{(2)}$
  - $x = \sqrt{3}$
  - $x = 8$
  - etc.
+ **Approximate** means there is **some** uncertainty.
  - $x \approx ...$
  - $x = 3 \pm 0.05$
  - $x = 4.8732...$
  - $x = ln(a) + E$ where $E$ is the error.

## Iterative Methods

There are cases where it may not be feasible nor convenient to analytically
determine solutions to problems. It may be possible to take an iterative
approach.
+ There will be parameters to tweak.
+ You need a threshold of "good enough" to stop the iteration.
+ An optimization target.
+ Iteratively tweak parameters such that your optimization target approaches
  its goal.
+ Each step, check if the threshold of "good enough" has been reached.
  - If so, stop.
  - If not, continue next step.
+ Unbounded iteration may occur, always put a maximum on the number of
  iterations.

