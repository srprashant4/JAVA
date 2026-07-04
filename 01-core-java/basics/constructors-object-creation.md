# Constructors and Object Creation

## Constructor
- Special block used to initialize an object.
- Constructor name is same as class name.
- It has no return type.
- If no constructor is written, Java provides a default constructor.

```java
class Employee {
    String name;

    Employee(String name) {
        this.name = name;
    }
}
```

## Constructor Overloading
- A class can have multiple constructors with different parameters.

```java
class User {
    User() {}

    User(String name) {}
}
```

## Constructor Chaining
- Calling one constructor from another constructor.
- Use `this()` for same class constructor.
- Use `super()` for parent class constructor.

```java
class User {
    User() {
        this("Guest");
    }

    User(String name) {}
}
```

## Object Creation Flow
1. Memory is allocated.
2. Fields get default values.
3. Parent constructor runs.
4. Instance variables and instance blocks run.
5. Current class constructor runs.

## Interview Point
- Constructors are not inherited.
- Constructor cannot be overridden.
- Constructor can be overloaded.
- First line of every constructor is either `this()` or `super()`. If not written, compiler adds `super()`.
