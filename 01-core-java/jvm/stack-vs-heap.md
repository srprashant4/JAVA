# Stack Vs Heap: (Refer to the memory-areas.md file for more information)

- Stack Memory:
    -  Stack is thread-specific, each thread has it's own stack.
    - It stores method calls in the form of stack frames.
    - Each frame has:
        - Local variables
        - Method Parameters
        - Return Addresses
    - Stack follows LIFO (Last In First Out)
    - It is automatically managed - memory is freed when method execution completes.
    - It is faster than the Heap memory.
    - If too many nested mathod calls happen (like infinite recursion), it results in: StackOverflowError

- Heap Memory:
    - Heap is shared among all the threads.
    - It stores Objects and Arrays
    - Memory allocation is dynamic.
    - Heap is managed by Garbage Collector (GC).
    - Objects that are no longer referenced are removed by the GC.
    - If Heap runs out of memory: OutOfMemoryError.