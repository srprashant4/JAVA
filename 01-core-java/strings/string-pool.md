# String Pool

## What Is String Pool?

- String Pool is a special memory area where Java stores string literals.
- It helps reuse common `String` objects instead of creating duplicate objects.
- String Pool is also called the String Constant Pool.
- It is managed by the JVM.

Example:

String s1 = "Java";
String s2 = "Java";

Here, only one `"Java"` object is created in the String Pool, and both `s1` and `s2` refer to the same object.

## Why String Pool Is Used

- Strings are heavily used in Java applications.
- Many string values are repeated, such as names, keys, messages, and configuration values.
- String Pool saves memory by reusing existing string objects.
- It improves performance because duplicate string literals do not need to be created again and again.

## String Literal Vs New String

### String Literal

String s1 = "Java";

- The JVM checks the String Pool.
- If `"Java"` already exists, the existing reference is returned.
- If it does not exist, a new string object is created in the pool.

### New String

String s2 = new String("Java");

- This creates a new `String` object in heap memory.
- The literal `"Java"` may still be stored in the String Pool.
- So this can create two objects if `"Java"` is not already present:
  - One in the String Pool
  - One in the heap

## Important Example

String s1 = "Java";
String s2 = "Java";
String s3 = new String("Java");

System.out.println(s1 == s2);      // true
System.out.println(s1 == s3);      // false
System.out.println(s1.equals(s3)); // true

Explanation:

- `s1` and `s2` refer to the same object in the String Pool.
- `s3` refers to a separate object in heap memory.
- `==` compares object references.
- `equals()` compares actual string content.

## `intern()` Method

- `intern()` is a method of the `String` class.
- It returns the reference of the string from the String Pool.
- If the string already exists in the pool, its existing reference is returned.
- If it does not exist, the string is added to the pool and its reference is returned.

Example:

String s1 = new String("Java");
String s2 = s1.intern();
String s3 = "Java";

System.out.println(s2 == s3); // true

Explanation:

- `s1` refers to a heap object.
- `s2` refers to the pooled `"Java"` object.
- `s3` also refers to the pooled `"Java"` object.

## String Pool Location

- Before Java 7, the String Pool was stored in PermGen.
- From Java 7 onward, the String Pool was moved to the heap.
- This makes string objects eligible for normal garbage collection.

## String Immutability And String Pool

- Strings are immutable in Java.
- Once a `String` object is created, its value cannot be changed.
- String Pool is possible because strings are immutable.
- If strings were mutable, changing one reference could affect other references pointing to the same pooled object.

Example:

String s1 = "Java";
String s2 = "Java";

If strings were mutable and `s1` changed `"Java"` to `"Python"`, then `s2` could also be affected. Java avoids this problem by making `String` immutable.

## Compile-Time Concatenation

String s1 = "Java";
String s2 = "Ja" + "va";

System.out.println(s1 == s2); // true

Explanation:

- `"Ja" + "va"` is resolved by the compiler at compile time.
- It becomes `"Java"`.
- Both `s1` and `s2` refer to the same pooled string.

## Runtime Concatenation

String s1 = "Java";
String part = "Ja";
String s2 = part + "va";

System.out.println(s1 == s2);      // false
System.out.println(s1.equals(s2)); // true

Explanation:

- `part + "va"` is evaluated at runtime.
- Runtime concatenation creates a new string object.
- The content is same, but the reference is different.

## Runtime Concatenation With `intern()`

String s1 = "Java";
String part = "Ja";
String s2 = (part + "va").intern();

System.out.println(s1 == s2); // true

Explanation:

- `part + "va"` creates a new string at runtime.
- `intern()` returns the pooled reference.
- So `s1` and `s2` point to the same object.

## `==` Vs `equals()`

| Comparison | Meaning |
| --- | --- |
| `==` | Compares object references |
| `equals()` | Compares actual string content |

Example:

String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);      // false
System.out.println(s1.equals(s2)); // true

## String Pool And Garbage Collection

- Since Java 7, the String Pool is stored in heap memory.
- Unused pooled strings can be garbage collected.
- However, string literals referenced by loaded classes usually remain reachable as long as the class is loaded.

## Common Interview Traps

### How many objects are created?

String s = new String("Java");

Answer:

- If `"Java"` is not already in the String Pool, two objects can be created:
  - One pooled string literal
  - One heap string object
- If `"Java"` is already in the pool, only one new heap object is created.

### What is the output?

String s1 = "Java";
String s2 = "Ja" + "va";
String s3 = new String("Java");

System.out.println(s1 == s2);
System.out.println(s1 == s3);
System.out.println(s1.equals(s3));

Output:

true
false
true

## Interview Points

- String Pool stores string literals to avoid duplicate string objects.
- String Pool improves memory usage and performance.
- String literals with the same value share the same pooled object.
- `new String("value")` creates a new object in heap memory.
- `==` checks whether two references point to the same object.
- `equals()` checks whether two strings have the same content.
- `intern()` returns the pooled version of a string.
- String Pool is possible because strings are immutable.
- Before Java 7, String Pool was in PermGen.
- From Java 7 onward, String Pool is stored in heap memory.
- Compile-time string concatenation uses the String Pool.
- Runtime string concatenation usually creates a new string object.

## Short Interview Answer

String Pool is a special memory area used by the JVM to store string literals. When the same string literal is used multiple times, Java reuses the existing object from the pool instead of creating a duplicate object. This saves memory and improves performance. String Pool works safely because strings are immutable. We can use `intern()` to get the pooled reference of a string.

## Example Follow-Up Questions

### Why is String immutable in Java?

String is immutable for security, thread safety, caching, hash code reuse, and String Pool safety.

### Is String Pool part of heap?

From Java 7 onward, String Pool is part of heap memory. Before Java 7, it was stored in PermGen.

### Does `new String("Java")` use the String Pool?

The literal `"Java"` is stored in the String Pool, but `new String("Java")` creates a separate object in heap memory.

### When should we use `intern()`?

Use `intern()` carefully when many duplicate string values exist and memory saving is important. Overusing it can increase String Pool pressure.
