# Polymorphism

Polymorphism means one name can have multiple forms.

## Compile-Time Polymorphism
- Also called method overloading.
- Resolved by compiler.
- Same method name, different parameter list.
- Return type alone cannot overload a method.

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

## Runtime Polymorphism
- Also called method overriding.
- Resolved at runtime based on actual object.
- Requires inheritance.
- Method signature must be same.

```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Test {
    public static void main(String[] args) {
        Animal animal = new Dog();
        animal.sound(); // Bark
    }
}
```

## Overloading vs Overriding

| Overloading | Overriding |
| --- | --- |
| Compile-time polymorphism | Runtime polymorphism |
| Same class or child class | Parent-child classes |
| Different parameters | Same method signature |
| Return type alone is not enough | Return type can be covariant |
| Static methods can be overloaded | Static methods cannot be overridden |

## Interview Point
- In `Animal animal = new Dog();`, method call depends on the actual object, not the reference type.
- Fields are not overridden; they are hidden.
