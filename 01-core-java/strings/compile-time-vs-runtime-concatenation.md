# Compile-Time vs. Runtime Concatenation

## Compile-Time Concatenation

```java
String s1 = "Java";
String s2 = "Ja" + "va";
System.out.println(s1 == s2); // true
```

Explanation:
- `"Ja" + "va"` is resolved by the compiler at compile time.
- It becomes `"Java"`.
- Both `s1` and `s2` refer to the same pooled string.

## Runtime Concatenation

```java
String s1 = "Java";
String s2 = new String("Ja") + new String("va");
System.out.println(s1 == s2); // false
```

Explanation:
- `"Ja" + "va"` is resolved at runtime.
- It creates a new String object.
- `s1` refers to a pooled string, while `s2` refers to a new string object.

## Runtime Concatenation With `intern()`

```java
String s1 = "Java";
String s2 = (new String("Ja") + new String("va")).intern();
System.out.println(s1 == s2); // true
```

Explanation:
- `new String("Ja") + new String("va")` creates a new string at runtime.
- `intern()` returns the pooled reference.
- So `s1` and `s2` point to the same object.

## `==` Vs `equals()`
| Comparison | Meaning |
| --- | --- |
| `==` | Compares object references |
| `equals()` | Compares actual string content |

Example:
```java
String s1 = "Java";
String s2 = new String("Java");
System.out.println(s1 == s2);      // false
System.out.println(s1.equals(s2)); // true
```

## String Pool And Garbage Collection
- Strings created at compile time are stored in the String Pool and are not eligible for garbage collection until the class is unloaded.
- Strings created at runtime (e.g., using `new String()`) are stored in the heap and are eligible for garbage collection when there are no references to them.
- Using `intern()` can help reduce memory usage by ensuring that only one instance of a string exists in the pool, but it should be used judiciously to avoid excessive memory consumption.

Example:
```java
final String s1 = "Java";
String s2 = "8";

System.out.println(s1 == "Java8"); // true
```

Explanation:
- Since `s1` is declared as `final`, the compiler can optimize the concatenation at compile time, resulting in a single pooled string `"Java8"`. Compiler treats `s1` as a constant, allowing it to perform the concatenation at compile time.
- So, `s1` + "8" is resolved to `"Java8"` at compile time, and both `s1` + "8" and `"Java8"` refer to the same pooled string.