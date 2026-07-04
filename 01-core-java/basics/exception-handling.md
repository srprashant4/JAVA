# Exception Handling

Exception handling is used to handle runtime problems without stopping the program abruptly.

## Exception Hierarchy

```text
Throwable
+-- Exception
|   +-- Checked Exception
|   +-- RuntimeException
+-- Error
```

## Checked Exception
- Checked at compile time.
- Must be handled using `try-catch` or declared using `throws`.
- Example: `IOException`, `SQLException`.

## Unchecked Exception
- Occurs at runtime.
- Not forced by compiler.
- Example: `NullPointerException`, `ArithmeticException`, `ArrayIndexOutOfBoundsException`.

## try-catch-finally

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
} finally {
    System.out.println("Always runs");
}
```

## throw vs throws

| throw | throws |
| --- | --- |
| Used to explicitly throw exception | Used to declare possible exception |
| Used inside method | Used in method signature |
| Throws one exception object | Can declare multiple exception types |

## final vs finally vs finalize

| Keyword | Meaning |
| --- | --- |
| `final` | Restricts variable, method or class |
| `finally` | Block that usually runs after try-catch |
| `finalize()` | Old GC-related method, not recommended |

## Interview Point
- `finally` usually runs, but not if JVM exits using `System.exit()`.
- `Error` should generally not be handled in application code.
