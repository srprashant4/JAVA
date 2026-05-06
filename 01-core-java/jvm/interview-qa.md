1) Why is Java slow as compared to C++?
-> Because Java uses interpretation and runtime compilation (JIT), unlike C++ which is directly compiled native code.

2) What is Method Area?
-> Method Area is a shared memory region in JVM that stores class-level information such as metadata, static variables, method bytecode, and runtime constant pool.
In Older JVMs: Method Area = PermGen
In newer JVMs: Method Area = Metaspace

3) What happens if Method Area is full?
-> It can lead to OutOfMemoryError: Metaspace

4) Why doesn’t Java allow direct memory access like C/C++?
-> Java does not allow direct memory access to ensure memory safety and security. Unlike C/C++, where pointers can lead to issues like buffer overflow or illegal memory access, Java uses managed memory through the JVM. The JVM enforces strict memory access rules, preventing programs from accessing memory outside their allocated space, which improves stability and avoids crashes.
-> This also enables features like automatic garbage collection and eliminates common bugs like dangling pointers.

5) Why do we get Metaspace OutOfMemoryError even if Heap memory is fine?
-> Metaspace OutOfMemoryError occurs when the JVM runs out of native memory allocated for class metadata. Since Metaspace is separate from Heap memory, it can become full even if Heap has available space. This typically happens due to excessive class loading, ClassLoader leaks, or dynamic class generation in frameworks like Spring.
-> Unlike Heap, Metaspace uses native memory, so its limit depends on system memory unless explicitly configured.

6) Where are Object References stored?
-> Object references are stored in the Stack, but the actual Objects are stored in the Heap.