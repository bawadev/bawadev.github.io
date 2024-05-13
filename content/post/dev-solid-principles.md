---
layout:     post
title:      "Solid Principles in Java"
subtitle:   "Understand the SOLID principles"
date:       2024-04-21
author:     "Pasindu"
image:      "https://www.freecodecamp.org/news/content/images/size/w2000/2020/08/solid.png"
summary: "The SOLID principles are a set of guidelines for writing object-oriented code that is maintainable, extensible, and easy to understand."
tags:
    - SOLID
    - Java
    - Design principles
---

The SOLID principles are a set of guidelines for writing object-oriented code that is maintainable, extensible, and easy to understand. These principles are:
1. Single Responsibility Principle (SRP)
2. Open/Closed Principle (OCP)
3. Liskov Substitution Principle (LSP)
4. Interface Segregation Principle (ISP)
5. Dependency Inversion Principle (DIP)
In this article, we will discuss each of these principles in detail, along with examples and explanations.
## Single Responsibility Principle (SRP)
The single responsibility principle states that a class should have only one reason to change. This means that a class should be responsible for only one task or set of related tasks. For example, a `Customer` class should not be responsible for handling orders and payments as well. Instead, it should focus on managing customer information.
Here's an example of how SRP can be applied in Java:
```java
public class Order {
    private int id;
    private String productName;
    private double price;
    
    public Order(int id, String productName, double price) {
        this.id = id;
        this.productName = productName;
        this.price = price;
    }
    
    // getters and setters for id, productName, and price
}
public class Payment {
    private int id;
    private double amount;
    private String paymentMethod;
    
    public Payment(int id, double amount, String paymentMethod) {
        this.id = id;
        this.amount = amount;
        this.paymentMethod = paymentMethod;
    }
    
    // getters and setters for id, amount, and paymentMethod
}
public class Customer {
    private int id;
    private String name;
    private List<Order> orders;
    private List<Payment> payments;
    
    public Customer(int id, String name) {
        this.id = id;
        this.name = name;
        this.orders = new ArrayList<>();
        this.payments = new ArrayList<>();
    }
    
    // getters and setters for id, name, orders, and payments
}
```
In this example, we have separated the responsibilities of managing orders and payments into separate classes (`Order` and `Payment`). This makes it easier to modify or extend these classes without affecting the functionality of the `Customer` class.
## Open/Closed Principle (OCP)
The open-closed principle states that a software entity (class, module, function, etc.) should be open for extension but closed for modification. This means that we should be able to add new functionality to a class without modifying its existing code. For example, we can add a new `calculateTotalPrice` method to the `Order` class without modifying its existing methods.
Here's an example of how OCP can be applied in Java:
```java
public abstract class Order {
    protected int id;
    protected String productName;
    protected double price;
    
    public Order(int id, String productName, double price) {
        this.id = id;
        this.productName = productName;
        this.price = price;
    }
    
    public abstract double calculateTotalPrice();
}
public class NormalOrder extends Order {
    private int quantity;
    
    public NormalOrder(int id, String productName, double price, int quantity) {
        super(id, productName, price);
        this.quantity = quantity;
    }
    
    @Override
    public double calculateTotalPrice() {
        return getQuantity() * getPrice();
    }
}
public class DiscountedOrder extends Order {
    private double discountPercentage;
    
    public DiscountedOrder(int id, String productName, double price, double discountPercentage) {
        super(id, productName, price);
        this.discountPercentage = discountPercentage;
    }
    
    @Override
    public double calculateTotalPrice() {
        return getPrice() * (1 - getDiscountPercentage());
    }
}
```
In this example, we have defined an abstract `Order` class with a `calculateTotalPrice` method. We can then create concrete subclasses (`NormalOrder` and `DiscountedOrder`) that extend the `Order` class and override the `calculateTotalPrice` method to add their own specific functionality.
## Liskov Substitution Principle (LSP)
The Liskov substitution principle states that objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. This means that a subclass should be able to be used in place of its parent class without any unexpected behavior. For example, we can use a `DiscountedOrder` object in place of a regular `Order` object without any issues.
Here's an example of how LSP can be applied in Java:
```java
public abstract class Order {
    protected int id;
    protected String productName;
    protected double price;
    
    public Order(int id, String productName, double price) {
        this.id = id;
        this.productName = productName;
        this.price = price;
    }
    
    public abstract void processOrder();
}
public class NormalOrder extends Order {
    private int quantity;
    
    public NormalOrder(int id, String productName, double price, int quantity) {
        super(id, productName, price);
        this.quantity = quantity;
    }
    
    @Override
    public void processOrder() {
        System.out.println("Processing normal order with ID: " + getId());
    }
}
public class DiscountedOrder extends Order {
    private double discountPercentage;
    
    public DiscountedOrder(int id, String productName, double price, double discountPercentage) {
        super(id, productName, price);
        this.discountPercentage = discountPercentage;
    }
    
    @Override
    public void processOrder() {
        System.out.println("Processing discounted order with ID: " + getId());
    }
}
```
In this example, we have defined an abstract `Order` class with a `processOrder` method. We can then create concrete subclasses (`NormalOrder` and `DiscountedOrder`) that extend the `Order` class and override the `processOrder` method to add their own specific functionality. Since both subclasses extend the `Order` class, they can be used interchangeably without any issues.
```java
public class Main {
    public static void main(String[] args) {
        Order order1 = new NormalOrder(1, "Product A", 10.0, 5);
        Order order2 = new DiscountedOrder(2, "Product B", 20.0, 10);
        
        List<Order> orders = Arrays.asList(order1, order2);
        
        for (Order o : orders) {
            o.processOrder();
        }
    }
}
```
In this example, we create a list of `Order` objects and pass it to a loop that calls the `processOrder` method on each order object. Since both `NormalOrder` and `DiscountedOrder` extend the `Order` class, they can be used interchangeably without any issues.
## Interface Segregation Principle (ISP)
The interface segregation principle states that a client should not be forced to implement methods it does not use. This means that we should create separate interfaces for related but distinct sets of functionality. For example, we can create a `Payment` interface with methods for processing payments and a `Shipping` interface with methods for shipping orders.
Here's an example of how ISP can be applied in Java:
```java
public interface Payment {
    void processPayment(double amount);
}
public class CreditCardPayment implements Payment {
    private String cardNumber;
    
    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void processPayment(double amount) {
        // Process credit card payment using card number
    }
}
public class PayPalPayment implements Payment {
    private String emailAddress;
    
    public PayPalPayment(String emailAddress) {
        this.emailAddress = emailAddress;
    }
    
    @Override
    public void processPayment(double amount) {
        // Process PayPal payment using email address
    }
}
public interface Shipping {
    void shipOrder(String orderNumber);
}
public class UPSShipping implements Shipping {
    private String trackingNumber;
    
    public UPSShipping(String trackingNumber) {
        this.trackingNumber = trackingNumber;
    }
    
    @Override
    public void shipOrder(String orderNumber) {
        // Ship order using UPS tracking number
    }
}
public class Main {
    public static void main(String[] args) {
        Payment payment1 = new CreditCardPayment("1234567890123456");
        Payment payment2 = new PayPalPayment("johndoe@email.com");
        
        Shipping shipping1 = new UPSShipping("ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890");
        Shipping shipping2 = new UPSShipping("ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890");
        
        System.out.println("Payment 1: " + payment1);
        System.out.println("Payment 2: " + payment2);
        System.out.println("Shipping 1: " + shipping1);
        System.out.println("Shipping 2: " + shipping2);
    }
}
```
In this example, we have defined separate `Payment` and `Shipping` interfaces with methods for processing payments and shipping orders. We can then create concrete implementations of these interfaces (`CreditCardPayment`, `PayPalPayment`, `UPSShipping`) that add their own specific functionality. Since each implementation only implements the methods it needs, we can use them interchangeably without any issues.
```java
public class Order {
    private int id;
    private String productName;
    private double price;
    
    public Order(int id, String productName, double price) {
        this.id = id;
        this.productName = productName;
        this.price = price;
    }
    
    public void setPayment(Payment payment) {
        this.payment = payment;
    }
    
    public void setShipping(Shipping shipping) {
        this.shipping = shipping;
    }
    
    private Payment payment;
    private Shipping shipping;
}
```
In this example, we have defined an `Order` class with a `setPayment` and `setShipping` method that allows us to set the payment and shipping for an order. Since both `Payment` and `Shipping` interfaces have only one method each, we can use them interchangeably without any issues.
```java
Order order1 = new Order(1, "Product A", 10.0);
order1.setPayment(payment1);
order1.setShipping(shipping1);
```
In this example, we create an `Order` object and set the payment and shipping for it using the appropriate implementations of the `Payment` and `Shipping` interfaces. Since both implementations only implement the methods they need, we can use them interchangeably without any issues.

Sure, here's an explanation of the Dependency Inversion Principle (DIP) from the SOLID principles in Markdown format:

## Dependency Inversion Principle (DIP)

The Dependency Inversion Principle (DIP) is one of the five SOLID principles of object-oriented programming. It states that:

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend on details. Details should depend on abstractions.

In simpler terms, this principle suggests that the relationship between software modules should be based on abstractions rather than concrete implementations. By doing so, the higher-level modules are decoupled from the lower-level modules, promoting flexibility and ease of maintenance.

Benefits of Dependency Inversion:
- Flexibility: Allows for easier changes and modifications in the system without affecting other parts of the codebase.
- Testability: Facilitates unit testing by enabling the use of mock objects or stubs for dependencies.
- Scalability: Helps in managing complexity and facilitating the addition of new features or components.

Example:

Consider a scenario where you have a `UserService` that needs to interact with a `Database` to fetch user data. Instead of directly depending on the concrete `Database` implementation, you can introduce an abstraction, such as an interface `IDatabase`, which both `UserService` and `Database` adhere to. This way, `UserService` depends on the abstraction (`IDatabase`), not on the concrete implementation (`Database`).

```csharp
// Abstraction
interface IDatabase {
    void Connect();
    void Disconnect();
    // Other methods...
}

// Concrete implementation
class Database : IDatabase {
    public void Connect() {
        // Connect to the database
    }
    public void Disconnect() {
        // Disconnect from the database
    }
    // Other methods...
}

// High-level module
class UserService {
    private readonly IDatabase _database;

    public UserService(IDatabase database) {
        _database = database;
    }

    // UserService methods using IDatabase abstraction
}
```

By adhering to the Dependency Inversion Principle, changes in the database implementation won't affect the `UserService` as long as the new implementation conforms to the `IDatabase` interface. This decoupling improves maintainability and facilitates easier code changes.

So this is about it. I hope you got an usderstanding on SOLID principles. Happy Coding!! 

---