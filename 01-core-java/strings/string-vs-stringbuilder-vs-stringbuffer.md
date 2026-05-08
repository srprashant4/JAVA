# String vs StringBuilder vs StringBuffer
- **String**: Immutable sequence of characters. Once created, it cannot be changed. Any modification results in a new String object.
- **StringBuilder**: Mutable sequence of characters. It can be modified after creation without creating new objects. Not thread-safe, but faster than StringBuffer.
- **StringBuffer**: Mutable sequence of characters. It can be modified after creation without creating new objects. Thread-safe, but slower than StringBuilder due to synchronization overhead.

## Performance Comparison
| Operation | String | StringBuilder | StringBuffer |
| --- | --- | --- | --- |   
| Concatenation | Creates new objects, slower | Modifies existing object, faster | Modifies existing object, slower due to synchronization |
| Thread Safety | Not thread-safe | Not thread-safe | Thread-safe |
| Memory Usage | More memory due to multiple objects | Less memory due to single object | Less memory due to single object |

## When to Use
- Use **String** when you have a fixed sequence of characters that won't change, such as constants or keys in a HashMap.
- Use **StringBuilder** when you need to perform multiple modifications to a string in a single thread, such as building a dynamic string or performing concatenation in a loop.
- Use **StringBuffer** when you need to perform multiple modifications to a string in a multi-threaded environment, where thread safety is a concern.

## Example
```java
String str = "Hello";
StringBuilder sb = new StringBuilder("Hello");
StringBuffer sbf = new StringBuffer("Hello");
str += " World"; // Creates new String object
sb.append(" World"); // Modifies existing StringBuilder object
sbf.append(" World"); // Modifies existing StringBuffer object
```

In this example, `str` creates a new String object when concatenated, while `sb` and `sbf` modify their existing objects without creating new ones.