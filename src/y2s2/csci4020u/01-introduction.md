# Introduction

## Human Computer Communication

Computation happens within an "environment".
+ **Runtime Environment** - An "environment" which supports the execution of
  a software program.
  - CLR, JRE, or broader including env vars, common libs, dir structure, etc.
  - Available/defined variables, functions, arrays, structs, and memory access
    for ex: C++.
+ A program/human must be able to communicate with the runtime environment.

### Communicating With Foundational Models

+ For a Turing Machine:
  - A tape with cells containing symbols.
  - A read/write head to access and update the tape.
  - Control logic for head movement and read/write ops.
  - Programmed via specifying the control logic.
+ Lambda Calculus:
  - Names, fn decleration, fn application.
  - Rewrite rules (alpha, beta, eta).
+ Human $\leftrightarrow$ Computer communication occurs during programming of
  these models.

### Some Runtime Environments.

+ C Programming Language:
  - Variables, functions, arrays, structs, direct memory access.
+ SQL:
  - Relational tables, First order logic.
  - Relational algebra with aggregation.

### Design Space

+ There is typically a tradeoff between expressiveness and intuitiveness of
  models and designs.

---

## Backend of Programming Languages
2 things of concern:
+ Runtime efficiency.
+ User experience.

### Why Not Use a Turing Machine as The Backend?

+ Too inefficient.
+ Lacks strong types.
+ Does not support modern processor's optimization features.

### JVM Bytecode

+ Simple instruction based runtime environment with:
  - An operand stack (32-bit or 64-bit cells).
    * 32-bits: integer, float, hot-spot reference.
    * 64-bits: long, double, reference.
  - A local variable array.
  - A heap for dynamic memory allocation.
  - Constant pool for storing constants.

**jasmin.jar** - Java "assembly" -> JVM Bytcode.
+ Can get from course jupyter server.

### Better Programming Interface Wanted!

A higher level abstraction (e.g. a programming language) can provide better:
+ Semantics: A better abstraction of a runtime environment.
+ Syntax: A better symbolic description of the computing process.
+ Verification: Safer communication to minimize human errors.
+ Compilation: Generate code into ex: Java Bytecode for faster execution.

### Notes

local var 0 is typically used for the `this` pointer.
+ Always `.limit locals` to n + 1 needed vars.

