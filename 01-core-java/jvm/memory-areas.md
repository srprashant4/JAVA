# JVM Memory Areas

## What Are JVM Memory Areas?

- JVM memory areas are runtime memory regions used by the Java Virtual Machine while executing a Java program.
- Each area has a specific purpose, such as storing objects, method calls, class metadata, or execution state.
- Understanding JVM memory areas is important for interviews, garbage collection, performance tuning, and debugging memory errors.

## Main JVM Memory Areas

The JVM runtime memory is mainly divided into:

- Heap
- Stack
- Method Area
- Metaspace
- PC Register
- Native Method Stack

## Heap Memory

- Heap is the runtime memory area where Java objects and arrays are stored.
- It is shared by all application threads.
- Garbage collection mainly works on heap memory.
- Heap size is controlled using:
  - `-Xms` for initial heap size
  - `-Xmx` for maximum heap size

Common heap error: java.lang.OutOfMemoryError: Java heap space

This happens when the application creates too many objects and the garbage collector cannot free enough heap memory.

## Stack Memory

- Each thread has its own JVM stack.
- Stack stores method call frames.
- Each method call creates a new stack frame.
- A stack frame stores:
  - Local variables
  - Method parameters
  - Return address
  - Operand stack
  - Intermediate calculation data

Common stack error: java.lang.StackOverflowError

This usually happens because of deep or infinite recursion.

## Method Area

- Method Area stores class-level structures.
- It contains runtime class metadata such as:
  - Class information
  - Method information
  - Field information
  - Runtime constant pool
  - Static variable metadata

- The Method Area is a logical JVM memory area.
- In Java 8 and later, method area implementation is mostly represented by Metaspace.

## Metaspace

- Metaspace stores class metadata.
- It was introduced in Java 8 and replaced PermGen.
- It uses native memory, not heap memory.
- It grows dynamically by default, depending on available native memory.
- It can be limited using:

-XX:MaxMetaspaceSize

Common metaspace error: java.lang.OutOfMemoryError: Metaspace

This usually happens when too many classes are loaded or class loaders are leaked.

## PC Register

- PC means Program Counter.
- Each thread has its own PC register.
- It stores the address of the current JVM instruction being executed.
- If the thread is executing a native method, the PC register value may be undefined.
- It helps the JVM manage thread execution and switching.

## Native Method Stack

- Native Method Stack is used for native methods written in languages like C or C++.
- Each thread may have its own native method stack.
- It supports execution of methods called through JNI.
- JNI stands for Java Native Interface.

Common native stack related error:

java.lang.StackOverflowError

or

java.lang.OutOfMemoryError

depending on the native stack usage and JVM implementation.

## Heap Vs Stack

| Feature | Heap | Stack |
| --- | --- | --- |
| Stores | Objects and arrays | Method call frames and local variables |
| Shared or private | Shared by all threads | One stack per thread |
| Memory management | Managed by garbage collector | Automatically cleared when method ends |
| Speed | Slower than stack | Faster than heap |
| Common error | `OutOfMemoryError: Java heap space` | `StackOverflowError` |
| Size options | `-Xms`, `-Xmx` | `-Xss` |

## Heap Vs Metaspace

| Feature | Heap | Metaspace |
| --- | --- | --- |
| Stores | Objects and arrays | Class metadata |
| Memory source | JVM heap memory | Native memory |
| Shared by threads | Yes | Yes |
| Garbage collection | Removes unreachable objects | Can unload classes when class loaders are unreachable |
| Common error | `OutOfMemoryError: Java heap space` | `OutOfMemoryError: Metaspace` |
| Tuning options | `-Xms`, `-Xmx` | `-XX:MetaspaceSize`, `-XX:MaxMetaspaceSize` |

## Runtime Constant Pool

- Runtime constant pool is part of the Method Area.
- It stores constants used by a class, such as:
  - String literals
  - Numeric literals
  - Symbolic references to methods and fields
- It is created when a class is loaded.
- It helps the JVM resolve references during execution.

## Thread Shared And Thread Private Areas

| Memory Area | Shared Or Private |
| --- | --- |
| Heap | Shared |
| Method Area | Shared |
| Metaspace | Shared |
| JVM Stack | Thread private |
| PC Register | Thread private |
| Native Method Stack | Thread private |

## Garbage Collection And JVM Memory Areas

- Garbage collection mainly cleans heap memory.
- Class metadata in metaspace can be cleaned only when the related class loader becomes unreachable.
- Stack memory is automatically cleaned when method execution completes.
- PC register memory is managed by the JVM and is thread specific.
- Native method stack cleanup depends on native method execution and JVM implementation.

## Important JVM Options

- `-Xms`
  - Sets initial heap size.

- `-Xmx`
  - Sets maximum heap size.

- `-Xss`
  - Sets stack size for each thread.

- `-XX:MetaspaceSize`
  - Sets the threshold at which GC is triggered for class metadata cleanup.

- `-XX:MaxMetaspaceSize`
  - Sets the maximum metaspace size.

## Common Memory Errors

| Error | Common Reason |
| --- | --- |
| `OutOfMemoryError: Java heap space` | Too many objects in heap |
| `StackOverflowError` | Infinite or deep recursion |
| `OutOfMemoryError: Metaspace` | Too many loaded classes or class loader leak |
| `OutOfMemoryError: GC overhead limit exceeded` | JVM spends too much time in GC but frees very little memory |
| `OutOfMemoryError: unable to create native thread` | Too many threads or insufficient native memory |

## Interview Points

- Heap stores objects and arrays.
- Stack stores method calls and local variables.
- Heap is shared among threads, but stack is thread private.
- Method Area stores class-level data and runtime constant pool.
- Metaspace is the Java 8 replacement for PermGen.
- Metaspace uses native memory, not heap memory.
- PC register stores the current instruction address for each thread.
- Native Method Stack supports execution of native methods through JNI.
- Garbage collection mainly works on heap memory.
- Stack memory is automatically freed when method execution completes.
- `StackOverflowError` is usually caused by deep recursion.
- `OutOfMemoryError: Java heap space` is caused by insufficient heap memory.
- `OutOfMemoryError: Metaspace` is often caused by excessive class loading or class loader leaks.

## Short Interview Answer

JVM memory is divided into several runtime areas. Heap stores objects and arrays and is shared by all threads. Stack stores method call frames and local variables and is private to each thread. The Method Area stores class-level data, while Metaspace stores class metadata in native memory from Java 8 onward. The PC register stores the current instruction address for each thread, and the Native Method Stack supports native method execution.

## Example Follow-Up Questions

### Which JVM memory areas are thread private?

JVM Stack, PC Register, and Native Method Stack are thread private.

### Which JVM memory areas are shared?

Heap, Method Area, and Metaspace are shared among threads.

### Is stack memory garbage collected?

No. Stack memory is automatically cleared when a method finishes execution. Garbage collection mainly manages heap memory.

### What is stored in heap memory?

Objects and arrays are stored in heap memory.

### What is stored in stack memory?

Method call frames, local variables, method parameters, operand stack, and intermediate execution data are stored in stack memory.

### Why does `StackOverflowError` occur?

It usually occurs due to infinite recursion or very deep method calls that exceed the thread stack size.
