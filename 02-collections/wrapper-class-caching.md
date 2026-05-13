# Wrapper Class Caching in Java

- In Java, wrapper classes (such as Integer, Long, Short, Byte, Character, Boolean) have a caching mechanism for certain values. This means that when you create an instance of a wrapper class with a value within a specific range, it may return a reference to a cached object instead of creating a new one. This is particularly true for Integer values between -128 and 127.

- The caching mechanism is implemented to improve performance and reduce memory usage for frequently used values. When you create an Integer object with a value within the cached range, it will return the same reference for that value. For example:

```java
Integer a = 100;
Integer b = 100;
System.out.println(a == b); // true
```
- In the above code, both `a` and `b` reference the same cached Integer object for the value 100, so `a == b` returns true.
- However, for values outside the cached range, new objects are created, and they do not reference the same memory location. For example:

```java
Integer x = 200;
Integer y = 200;
System.out.println(x == y); // false
```

- In this case, `x` and `y` reference different Integer objects for the value 200, so `x == y` returns false.
- This caching behavior is specific to wrapper classes and does not apply to custom objects unless we implement caching ourselves. It is important to be aware of this behavior when comparing wrapper class instances using `==`, as it may lead to unexpected results if you are not considering the caching mechanism. Always use `.equals()` for logical equality checks instead of `==` when working with wrapper classes or custom objects.

- For Long, Short, Byte, Character, and Boolean wrapper classes, similar caching mechanisms exist for specific ranges of values. For example, Long caches values from -128 to 127, Short caches values from -128 to 127, Byte caches values from -128 to 127, Character caches values from '\u0000' to '\u007F', and Boolean caches the values true and false.

- Example for Boolean caching:

```java
Boolean bool1 = true;
Boolean bool2 = true;
System.out.println(bool1 == bool2); // true
```

- In this example, both `bool1` and `bool2` reference the same cached Boolean object for the value true, so `bool1 == bool2` returns true.

- It is important to note that this caching mechanism is an implementation detail of the Java language and may vary between different versions of Java or different JVM implementations. Therefore, it is always recommended to use `.equals()` for comparing wrapper class instances to ensure correct behavior regardless of caching.