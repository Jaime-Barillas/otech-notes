# Semantics and Syntax of Higher Level Languages

Syntax is the main medium of communication between the dev and the backend.

+ Data abstraction - Concerns itself with how data is described.
+ Runtime abstraction/code execution - Concerns itself with how the data is
  processed.

## Data Abstraction

Aides in describing data.
+ Syntax.
+ Type system.

+ **Symbol Names** - Semantic names can be given to data.
+ **Type Annotations** - Can annotate values with type information to make
  verification possible.

### Rich Data Structures

Backends like the JVM runtime have memory designs (stack, local vars, heap)
oriented towards performance. Programming languages have the oportunity to
offer richer data structures to express memory layouts that suit the needs of
the application.

+ Arrays.
+ Dictionaries.
+ Strongly typed data structures.
+ Aggregates:
  - Records, structs, etc.

### Advanced Typing Features

+ Dependent typing.
+ TODO: List more examples.

## Runtime Abstraction

How data is processed and how code is executed.
+ Scopes, modules, functions.
+ Data types.
+ Concurrency.
+ etc.

+ Values are assigned to variables.
+ Variables are valid within defined scopes.
  - Scopes can be nested.

## Multicore Architecture

+ Languages can take a structured approach to defining code that runs in
  multiple threads. See Kotlin's `kotlin.concurrent.thread` package or Go's
  coroutines and channels.
+ Threaded execution has non-deterministic order.
+ Languages often provide ways to have inter-thread communication.

