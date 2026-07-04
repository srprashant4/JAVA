# OOPs Principles

OOPs helps model real-world problems using objects that contain data and behavior.

## Class and Object
- Class is a blueprint.
- Object is an instance of a class.

```java
class Car {
    String brand;

    void drive() {
        System.out.println("Driving");
    }
}
```

## Encapsulation
- Wrapping data and methods together inside a class.
- Usually achieved using private fields and public getters/setters.
- Protects data from direct unwanted access.

```java
class Account {
    private double balance;

    public double getBalance() {
        return balance;
    }
}
```

## Inheritance
- One class can reuse fields and methods of another class using `extends`.
- Supports code reuse and method overriding.
- Java supports single inheritance for classes.

```java
class Animal {
    void eat() {}
}

class Dog extends Animal {
    void bark() {}
}
```

## Polymorphism
- Same method name behaves differently depending on object or arguments.
- Two types: compile-time polymorphism and runtime polymorphism.

## Abstraction
- Hiding implementation details and showing only essential behavior.
- Achieved using abstract classes and interfaces.

## Interview Point
- Encapsulation protects data.
- Inheritance reuses code.
- Polymorphism provides flexibility.
- Abstraction hides complexity.
