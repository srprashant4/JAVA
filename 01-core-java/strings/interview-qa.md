# Java Strings - Interview Q&A

## Why are Strings immutable in java?
- Strings are immutable in java to ensure security, thread-safety and performance optimization through String Pooling, and reliable hashing behavior for collections like hashmap.

## What is String Constant Pool?
- String Constant Pool is a special memory area inside heap where Java stores literals to resuse identical values and optimize memory usage.

## Difference between
```java
String s1 = "Java";
String s2 = new String("Java");
```

- `s1` refers to a string literal in the String Pool, while `s2` refers to a new String object in heap memory.
- `s1 == s2` is false because they refer to different objects, but `s1.equals(s2)` is true because they have the same content.

## == Vs equals() for String comparison?
- `==` compares object references (memory addresses), while `equals()` compares the actual content of the strings. For string comparison, `equals()` should be used to check for value equality.

## Why is StringBuilder faster than String?
- StringBuilder is faster than String because it is mutable and can modify the existing object without creating new ones, while String creates a new object for every modification, leading to increased memory usage and slower performance.

## Difference between StringBuilder and StringBuffer?
| StringBuilder | StringBuffer |
| --- | --- |
| Not thread-safe | Thread-safe |
| Faster due to lack of synchronization | Slower due to synchronization overhead |
| Suitable for single-threaded scenarios | Suitable for multi-threaded scenarios |

## Why does this print true?
```java
final String s1 = "Java";
String s2 = s1 + "8";
System.out.println(s2 == "Java8"); // true
```
- Since `s1` is declared as `final`, the compiler can optimize the concatenation at compile time, resulting in a single pooled string `"Java8"`. Compiler treats `s1` as a constant, allowing it to perform the concatenation at compile time.

## Difference between compile time and runtime concatenation of strings?
- Compile-time concatenation is resolved by the compiler and results in a single string literal in the String Pool, while runtime concatenation creates a new String object on the heap.

## Can String Pool objects be garbage collected?
- String Pool objects created at compile time are not eligible for garbage collection until the class is unloaded, while strings created at runtime (e.g., using `new String()`) are eligible for garbage collection when there are no references to them.

## Can String Pool cause memory leaks?
- String Pool itself does not cause memory leaks, but excessive use of `intern()` can lead to memory issues if too many strings are interned, as they will remain in the pool and cannot be garbage collected until the class is unloaded.