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
- It is created when a class is loaded.
- It works like a per-class runtime lookup table used by the JVM while executing bytecode.
- It stores entries used by a class, such as:
  - Numeric literals
  - Class names
  - Method names and method references
  - Field names and field references
  - Symbolic references used by bytecode instructions
- It helps the JVM resolve symbolic references into actual runtime references.

Important point about string literals:

- The runtime constant pool may contain an entry for a string literal such as `"Riya"`.
- The actual interned `String` object is stored in the String Constant Pool.
- In modern Java, the String Constant Pool is stored on the heap.
- So, Metaspace/runtime constant pool knows about the literal, but the actual `String` object lives in the heap-based String Constant Pool.

Example:

Account a = new Account("Riya", 100);
a.deposit(50);

The bytecode may refer to runtime constant pool entries for:

- The class name `Account`
- The constructor reference `Account.<init>`
- The method reference `Account.deposit`
- The string literal entry for `"Riya"`

The JVM uses these entries to resolve what class, method, field, or literal the bytecode is talking about.

## How JVM Memory Areas Work Together During Execution

For a method call such as:

a.deposit(50);

The flow is:

1. PC Register points to the current bytecode instruction, such as `invokevirtual deposit`.
2. The current stack frame contains local variables, including reference variable `a`.
3. Reference variable `a` points to the actual `Account` object in the heap.
4. The heap object has a link to its class metadata in Metaspace.
5. Metaspace tells the JVM the field layout, method information, bytecode, and runtime constant pool for `Account`.
6. The JVM pushes a new stack frame for `deposit`.
7. Inside that frame, `this` points to the same heap object and `amount` stores `50`.
8. The method bytecode updates the heap object, for example changing `balance` from `100` to `150`.
9. When the method returns, the `deposit` stack frame is popped and the PC register moves to the next instruction in the caller.

Important links:

- PC Register points to the current bytecode instruction for a thread.
- Stack frames store local variables, parameters, operand stack data, and references.
- Stack references point to heap objects.
- Heap objects store instance data and link to class metadata.
- Metaspace stores class metadata, method metadata, field metadata, and runtime constant pool data.
- Runtime constant pool helps resolve symbolic references used by bytecode.
- The actual interned string objects are stored in the heap-based String Constant Pool in modern Java.

Simple chain:

PC Register
  -> current bytecode instruction

Stack Frame
  -> local variable reference

Heap
  -> actual object data

Metaspace
  -> class metadata, method info, field layout, runtime constant pool

Another way to remember it:

PC register decides what instruction runs next.
Stack frame gives the JVM local variables and references.
Heap contains the actual objects.
Metaspace explains what those objects are and how their methods/fields work.

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
- PC register points to bytecode instructions, not objects.
- Stack references point to heap objects.
- Heap objects link to class metadata in Metaspace.
- Runtime constant pool stores per-class symbolic references used by bytecode.
- String Constant Pool stores actual interned `String` objects on the heap in modern Java.
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
