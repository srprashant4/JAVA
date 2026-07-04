# Core Java Concepts

## Java is Pass by Value
- Java always passes a copy of the value.
- For objects, the copied value is the reference.
- A method can modify the object's internal state, but cannot replace the caller's reference.

```java
static void changeName(Student s) {
    s.name = "Rahul";
}
```

## Primitive vs Reference Types

| Primitive | Reference |
| --- | --- |
| Stores actual value | Stores reference to object |
| Examples: `int`, `double`, `boolean` | Examples: `String`, arrays, custom classes |
| Cannot be null | Can be null |

## Access Modifiers

| Modifier | Access |
| --- | --- |
| `private` | Same class only |
| default | Same package |
| `protected` | Same package and subclasses |
| `public` | Everywhere |

## == vs equals()
- `==` compares primitive values or object references.
- `equals()` compares object content if properly overridden.

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);      // false
System.out.println(a.equals(b)); // true
```

## main Method

```java
public static void main(String[] args)
```

- `public`: JVM can access it.
- `static`: JVM can call it without creating object.
- `void`: does not return value.
- `String[] args`: command-line arguments.

## Interview Point
- Java is not pass by reference.
- Overriding `equals()` usually requires overriding `hashCode()`.
- Default values are assigned to fields, not local variables.
