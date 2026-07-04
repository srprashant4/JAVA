# Abstraction, Interface and Abstract Class

Abstraction hides internal implementation and exposes only required behavior.

## Abstract Class
- Declared using `abstract`.
- Can have abstract and concrete methods.
- Can have constructors, instance variables and static methods.
- Used when related classes share common state or behavior.

```java
abstract class Vehicle {
    abstract void start();

    void stop() {
        System.out.println("Stopped");
    }
}
```

## Example of Abstract Class

```java
public abstract class PaymentProcessor {

    // Shared state
    protected String customerName;
    protected String orderId;

    public PaymentProcessor(String customerName, String orderId) {
        this.customerName = customerName;
        this.orderId = orderId;
    }

    // Common implementation
    public void validateRequest() {
        System.out.println("Validating order " + orderId);
    }

    public void authenticateCustomer() {
        System.out.println("Authenticating " + customerName);
    }

    public void generateReceipt() {
        System.out.println("Receipt Generated");
    }

    public void saveAuditLogs() {
        System.out.println("Saving audit logs...");
    }

    // Child MUST implement
    public abstract void processPayment();

}
```

- The `PaymentProcessor` class defines common behavior for all payment processors, such as validating requests, authenticating customers, generating receipts, and saving audit logs. However, the actual payment processing logic is left abstract, allowing subclasses to provide their own implementation.
- The process of payment is specific to each payment method, so subclasses like `CreditCardPaymentProcessor` and `UPIPaymentProcessor` will implement the `processPayment()` method according to their respective payment processing logic.
- Example implementation of a concrete subclass:

```java
public class CreditCardPaymentProcessor extends PaymentProcessor {
    
    public CreditCardPaymentProcessor(String customerName, String orderId) {
        super(customerName, orderId);
    }

    @Override
    public void processPayment() {
        System.out.println("Processing credit card payment for " + customerName);
        // Credit card payment processing logic here
    }
}
```

- If in future, we want to add another payment method, we can create a new subclass of `PaymentProcessor` without modifying the existing code, adhering to the Open/Closed Principle.

- Also, if we need to add a new feature for refund processing, we have to create a new interface `Refundable` and implement it in the relevant payment processors, ensuring that we don't modify existing classes, thus maintaining backward compatibility. This is also because not all payment processors may support refunds, so we can selectively implement the `Refundable` interface in those that do.
- Example of `Refundable` interface:

```java
public interface Refundable {
    void processRefund(double amount);
}
```

- Example implementation of a concrete subclass that supports refunds:

```java
public class UpiPaymentProcessor extends PaymentProcessor implements Refundable {
    
    public UpiPaymentProcessor(String customerName, String orderId) {
        super(customerName, orderId);
    }

    @Override
    public void processPayment() {
        System.out.println("Processing UPI payment for " + customerName);
        // UPI payment processing logic here
    }

    @Override
    public void processRefund(double amount) {
        System.out.println("Processing refund of " + amount + " for " + customerName);
        // UPI refund processing logic here
    }
}
```

## Interface
- Defines a contract that classes must follow.
- Methods are public and abstract by default.
- Variables are public, static and final by default.
- Supports default and static methods.
- A class can implement multiple interfaces.

```java
interface Payment {
    void pay(double amount);
}

class UpiPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid: " + amount);
    }
}
```

- Example of interface having default and static methods:

```java
interface Payment {
    void pay(double amount);
    default void logPayment(double amount) {
        System.out.println("Payment of " + amount + " logged.");
    }
    static void printPaymentInfo() {
        System.out.println("Payment interface provides payment capabilities.");
    }
}
class UpiPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid: " + amount);
        logPayment(amount);
    }
}
public class Main {
    public static void main(String[] args) {
        Payment.printPaymentInfo();
        Payment payment = new UpiPayment();
        payment.pay(100.0);
    }
}
```

## Why are the concrete methods in interface always defined as default?
- To provide a default implementation for the method, so that classes implementing the interface are not forced to provide their own implementation unless they want to override it. This allows for backward compatibility when new methods are added to the interface, as existing classes that implement the interface will not break due to missing method implementations. It also allows for code reuse and reduces duplication, as common behavior can be defined in the interface itself, rather than requiring each implementing class to define it separately.

- Practical example is when java 8 introduced the `Stream` API, which added several new methods to the `Collection` interface. If these methods were not defined as default, all existing classes that implemented `Collection` would have to provide their own implementation of these new methods, which would break backward compatibility. By defining them as default, existing classes can continue to work without modification, while still allowing new classes to take advantage of the new functionality.

## Abstract Class vs Interface

| Abstract Class | Interface |
| --- | --- |
| Used for common base behavior | Used for contract/capability |
| Can have instance variables | Only constants |
| Can have constructors | Cannot have constructors |
| Class can extend only one | Class can implement many |

## When to Use
- Use abstract class when classes are closely related.
- Use interface when unrelated classes need the same capability.

Example: `Dog` and `Cat` can extend `Animal`; `Car` and `CardPayment` can both implement `Payable` only if both truly support payment behavior.
