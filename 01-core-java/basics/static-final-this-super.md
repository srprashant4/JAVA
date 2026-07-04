# static, final, this and super

## static
- Belongs to class, not object.
- Shared by all objects.
- Static methods cannot directly access non-static members.
- Static methods are hidden, not overridden.

```java
class Counter {
    static int count;

    Counter() {
        count++;
    }
}
```

## final
- `final` variable: value cannot be changed.
- `final` method: cannot be overridden.
- `final` class: cannot be inherited.

```java
final class Constants {
    static final double PI = 3.14;
}
```

## this
- Refers to current object.
- Used to resolve variable shadowing.
- Can call another constructor using `this()`.

```java
class Student {
    String name;

    Student(String name) {
        this.name = name;
    }
}
```

## super
- Refers to parent class object.
- Used to call parent method, constructor or variable.
- `super()` must be the first statement in constructor.

```java
class Dog extends Animal {
    Dog() {
        super();
    }
}
```

## Interview Point
- `static final` is commonly used for constants.
- `this()` and `super()` cannot both be first statements in the same constructor.
