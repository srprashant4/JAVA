# JVM Metaspace

## What Is Metaspace?

- Metaspace is the JVM memory area used to store class metadata.
- It was introduced in Java 8 as a replacement for PermGen.
- It stores information such as:
  - Class names
  - Method metadata
  - Field metadata
  - Constant pool data
  - Runtime annotations
  - Method bytecode metadata
  - Class loader related metadata

## Why Metaspace Replaced PermGen

- Before Java 8, class metadata was stored in PermGen, which was part of the JVM heap.
- PermGen had a fixed maximum size and often caused `java.lang.OutOfMemoryError: PermGen space`.
- Metaspace uses native memory instead of heap memory.
- This makes class metadata storage more flexible because it can grow dynamically based on application needs.

## Where Metaspace Is Located

- Metaspace is not stored inside the Java heap.
- It is allocated from native memory, which is memory managed by the operating system.
- Although it is outside the heap, it is still managed by the JVM.

## Important JVM Options

- `-XX:MetaspaceSize`
  - Initial threshold at which garbage collection is triggered to unload classes.
  - It is not the starting allocated size in the same simple way as heap `-Xms`.

- `-XX:MaxMetaspaceSize`
  - Sets the maximum limit for metaspace.
  - If this limit is reached and the JVM cannot free class metadata, it throws an error.

- `-XX:CompressedClassSpaceSize`
  - Used when compressed class pointers are enabled.
  - Controls memory reserved for class metadata related to compressed class pointers.

## Common Error

java.lang.OutOfMemoryError: Metaspace

This usually happens when:

- Too many classes are loaded.
- Class loaders are not being garbage collected.
- The application dynamically generates many classes.
- There is a class loader leak.
- Frameworks, proxies, reflection, or bytecode generation libraries create excessive metadata.

## Metaspace And Garbage Collection

- Class metadata can be freed when the corresponding class loader becomes unreachable.
- Classes are usually unloaded during garbage collection.
- If a class loader is still referenced, all classes loaded by it remain in metaspace.
- Class loader leaks are a common reason for metaspace memory issues.

## When does a ClassLoader become Unavailable?

- A classloader in Java is like a "worker" that loads classes (blueprints for objects) into the     JVM's memory, specifically into an area called Metaspace where class details are stored.

- What Does "Unavailable" Mean?
  - "Unavailable" simply means the classloader is no longer accessible or usable by the program. It's like the worker has been "let go" and can't do any more work. This happens when the JVM's garbage collector (a cleanup process) decides the classloader is no longer needed and removes it from memory.
- When Does a Classloader Become Unavailable?
  - A classloader becomes unavailable (eligible for garbage collection) when:
    a) No references exist: Nothing in your running program (like objects, threads, or other code) is still pointing to or using that classloader. For example, if you have a web application that uses its own classloader, and you shut down or redeploy the app, the classloader might become unreachable.
    b) During garbage collection: The JVM periodically cleans up unused memory. If the classloader has no active references, it gets collected. Once this happens, the classes it loaded can also be unloaded from Metaspace, freeing up space.
    c) Common triggers: This often occurs in dynamic environments like application servers (e.g., Tomcat), where classloaders are created for each app. If an app is unloaded or reloaded, its classloader can become unavailable. Leaks (where references are accidentally kept) prevent this, leading to Metaspace memory issues.
  - In short, it's the JVM's way of cleaning house—removing loaders that aren't needed so memory doesn't get clogged with unused class data. If classloaders stick around too long (due to leaks), you might see OutOfMemoryError: Metaspace errors.

## Metaspace Vs Heap

| Feature | Heap | Metaspace |
| --- | --- | --- |
| Stores | Objects and arrays | Class metadata |
| Memory source | JVM heap memory | Native memory |
| Tuned by | `-Xms`, `-Xmx` | `-XX:MetaspaceSize`, `-XX:MaxMetaspaceSize` |
| Error | `OutOfMemoryError: Java heap space` | `OutOfMemoryError: Metaspace` |
| Garbage collection | Removes unreachable objects | Can unload classes when class loaders are unreachable |

## Metaspace Vs PermGen

| PermGen | Metaspace |
| --- | --- |
| Used before Java 8 | Introduced in Java 8 |
| Stored class metadata in heap-like JVM memory | Stores class metadata in native memory |
| Had fixed size limits | Grows dynamically by default |
| Could throw `OutOfMemoryError: PermGen space` | Can throw `OutOfMemoryError: Metaspace` |
| Tuned using `-XX:PermSize` and `-XX:MaxPermSize` | Tuned using `-XX:MetaspaceSize` and `-XX:MaxMetaspaceSize` |

## Interview Points

- Metaspace stores class-level metadata, not normal Java objects.
- It is outside the Java heap and uses native memory.
- It replaced PermGen starting from Java 8.
- By default, metaspace can grow until the system's available native memory is exhausted.
- In production, it is common to set `-XX:MaxMetaspaceSize` to avoid unlimited native memory growth.
- Increasing metaspace size may hide the problem temporarily, but the real issue may be a class loader leak.
- Class unloading depends on class loader garbage collection.
- Applications using dynamic class generation, such as Spring, Hibernate, CGLIB, Byte Buddy, or application servers, may use more metaspace.

## Short Interview Answer

Metaspace is a native memory area introduced in Java 8 to store class metadata. It replaced PermGen, which had fixed size limitations and often caused PermGen out-of-memory errors. Since metaspace uses native memory, it can grow dynamically, but it can still throw `OutOfMemoryError: Metaspace` if class metadata grows too much or class loaders are leaked. It can be tuned using `-XX:MetaspaceSize` and `-XX:MaxMetaspaceSize`.

## Example Follow-Up Questions

### Why can metaspace cause memory leaks?

Metaspace itself does not usually leak. The common issue is a class loader leak. If a class loader remains reachable, all classes loaded by it also remain reachable, so their metadata cannot be removed from metaspace.

### Is metaspace part of heap?

No. Metaspace is outside the Java heap and uses native memory.

### What happens if metaspace is full?

The JVM tries to trigger garbage collection and unload unused classes. If enough metadata cannot be freed, the JVM throws `java.lang.OutOfMemoryError: Metaspace`.
