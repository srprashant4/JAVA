# Core Java Basics - Interview Q&A

## What are the four OOPs principles?
- Encapsulation, inheritance, polymorphism and abstraction.

## What is encapsulation?
- Binding data and methods together and restricting direct access using private fields and public methods.

## What is inheritance?
- Acquiring properties and behavior of a parent class using `extends`.

## What is polymorphism?
- Same method name behaving differently based on arguments or actual object.

## Compile-time vs runtime polymorphism?
| Compile-time | Runtime |
| --- | --- |
| Method overloading | Method overriding |
| Resolved by compiler | Resolved by JVM at runtime |
| Does not require inheritance | Requires inheritance |

## Can static methods be overridden?
- No. Static methods belong to the class, so they are hidden, not overridden.

## Can constructors be inherited or overridden?
- No. Constructors are not inherited and cannot be overridden. They can be overloaded.

## Abstract class vs interface?
- Abstract class is used for shared base behavior.
- Interface is used for a contract or capability.
- A class can extend one abstract class but implement multiple interfaces.

## Is Java pass by value or pass by reference?
- Java is always pass by value. For objects, the value passed is a copy of the reference.

## Difference between `throw` and `throws`?
- `throw` is used to throw an exception object.
- `throws` is used in method signature to declare exceptions.

## Difference between checked and unchecked exceptions?
- Checked exceptions are checked at compile time and must be handled or declared.
- Unchecked exceptions occur at runtime and are not forced by compiler.

## Difference between `final`, `finally` and `finalize()`?
- `final` restricts modification, overriding or inheritance.
- `finally` is a block used with exception handling.
- `finalize()` was related to garbage collection and is not recommended.

## What is the use of `super`?
- `super` is used to access parent class constructor, method or variable.

## What is the use of `this`?
- `this` refers to the current object.
