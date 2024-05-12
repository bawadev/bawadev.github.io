---
layout:     post
title:      "Java Lambda: All you need to know to code"
subtitle:   "The most practical way to understand Java Lambda"
date:       2024-01-09
author:     "Pasindu"
image:      "https://www.educative.io/v2api/editorpage/5947956966981632/image/5758486424584192"
summary: "Java lambdas provide a concise way to express instances of single-method interfaces (functional interfaces). They enhance code readability and maintainability by allowing you to write more compact and expressive code for functional programming constructs. Lambdas also facilitate the use of functional interfaces in the Java API, making it easier to work with collections and streams. Overall, they contribute to more efficient and modern Java programming practices."
tags:
    - JAVA
    - JAVA LAMBDA
    - DEVELOPMENTS
    - functional interfaces
    
---

Java lambdas provide a concise way to express instances of single-method interfaces (functional interfaces). They enhance code readability and maintainability by allowing you to write more compact and expressive code for functional programming constructs. Lambdas also facilitate the use of functional interfaces in the Java API, making it easier to work with collections and streams. Overall, they contribute to more efficient and modern Java programming practices.

## Features

#### 1. Functional Interface:

```java
// Before lambda expressions
interface MyFunction {
    void doSomething();
}

// Using lambda expression
MyFunction myFunction = () -> System.out.println("Doing something");
```
#### 2. List Iteration:

```java
List<String> fruits = Arrays.asList("Apple", "Orange", "Banana");

// Before lambda expressions
for (String fruit : fruits) {
    System.out.println(fruit);
}

// Using lambda expression with forEach
fruits.forEach(fruit -> System.out.println(fruit));
```
#### 3. Comparator:

```java
List<String> names = Arrays.asList("John", "Alice", "Bob");

// Before lambda expressions
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }
});

// Using lambda expression with Comparator
Collections.sort(names, (s1, s2) -> s1.compareTo(s2));
```
#### 4. Runnable:
```java
// Before lambda expressions
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running task");
    }
};

// Using lambda expression with Runnable
Runnable lambdaRunnable = () -> System.out.println("Running task");
```
These examples demonstrate how lambda expressions simplify code for functional interfaces, iteration, comparison, and defining simple tasks.

### Influence of Java Script on Lambda
So, you know how JavaScript is all about those short and sweet anonymous functions and closures, right? Well, turns out Java took a cue from that vibe and introduced lambda expressions, making Java code a lot more concise and readable. Plus, since JavaScript is big on functional programming, Java wanted in on the action, so they added lambdas to make Java more flexible and expressive. And hey, you know those arrow functions in JavaScript? Java's lambdas kinda rock a similar vibe, making code look snappier and cooler. Basically, Java looked at JavaScript, thought, "Hey, that's pretty cool," and decided to spruce itself up, making it more competitive and fun for devs who dig functional programming.


### What are the things should know when writing lambda
When writing lambda expressions in Java, there are several important considerations to keep in mind. 

#### 1. Functional Interfaces:
Lambdas are used with functional interfaces (interfaces with a single abstract method). Ensure that the interface you are working with is a valid functional interface.
```java
// Functional interface
@FunctionalInterface
interface MyFunction {
    void myMethod(String s);
}
MyFunction myFunction = s -> System.out.println(s);
```
#### 2. Lambda Syntax:
Understand the basic syntax of lambda expressions: (parameters) -> expression or (parameters) -> { code block }. Pay attention to parameter types, parentheses, the arrow (->) symbol, and return types.
```java
// Single expression lambda
(x, y) -> x + y

// Code block lambda
(x, y) -> {
    int result = x + y;
    return result;
}
```
#### 3. Type Inference:
Java can infer the types of parameters in many cases. However, in complex situations, you may need to specify parameter types explicitly. For example: (String s) -> System.out.println(s).
```java
// Type inference
(String s) -> System.out.println(s);

// Explicit type
(int x, int y) -> x + y
```

#### 4. Effectively Final Variables:
Variables used inside a lambda expression should be effectively final, meaning their values should not change after being assigned. This is a requirement for using local variables in lambdas.
```java
// Effectively final
int x = 10;
MyFunction myFunction = s -> System.out.println(s + x);
```

#### 5. No Checked Exceptions:
Lambdas cannot throw checked exceptions unless the corresponding exception is declared in the functional interface. Be mindful of exception handling within lambdas.
```java
// Lambda with checked exception
interface MyInterface {
    void myMethod() throws Exception;
}

// Correct lambda usage
MyInterface myInterface = () -> {
    // code that might throw an exception
};
```

#### 6. Return Statement:
Understand how the return statement works in lambdas. If your lambda has a single expression, the result is automatically returned. If you use a code block, you need to use the return statement explicitly.
```java
// Single expression lambda
(x, y) -> x + y

// Code block lambda with explicit return
(x, y) -> {
    return x + y;
}
```

#### 7. Enclosing Scope:
Lambdas capture variables from their enclosing scope. Be aware of the implications, especially if you're using lambdas in the context of streams or parallel processing.
```java
int multiplier = 2;

// Lambda accessing variable from enclosing scope
MyFunction myFunction = s -> System.out.println(s + multiplier);
```

#### 8. Method References:
Familiarize yourself with method references, which provide a shorthand syntax for lambda expressions. They can make your code more concise and readable.
```java
// Method reference to a static method
MyFunction myFunction = MyClass::staticMethod;

// Method reference to an instance method
MyFunction myFunction = myObject::instanceMethod;
```

#### 9. Streams and Collections:
Lambdas are often used with Java streams and collections. Understand how to use them in these contexts to perform operations like filtering, mapping, and reducing.
```java
List<String> names = Arrays.asList("John", "Alice", "Bob");

// Using lambda with stream and filter
names.stream()
     .filter(name -> name.startsWith("A"))
     .forEach(System.out::println);
```

#### 10. Readability and Style:
Prioritize readability. While lambdas can make code concise, overly complex expressions may reduce readability. Use lambdas judiciously and consider breaking down complex logic into separate methods if needed.
By keeping these considerations in mind, you can write effective and clean lambda expressions in Java, taking advantage of the language's functional programming features.
```java
// Complex lambda expression
MyFunction myFunction = (x, y) -> x > 0 ? x + y : x - y;

// Improved readability by using a method
MyFunction myFunction = (x, y) -> calculateResult(x, y);

private int calculateResult(int x, int y) {
    return x > 0 ? x + y : x - y;
}
```
### Let's go with an Example

1. Define a Functional Interface:
```java
@FunctionalInterface
interface MyLambda {
    void myFunction(String message);
}
```
Here, we've created a functional interface named MyLambda with a single abstract method myFunction.

2. Use Lambda Expression:
```java
public class LambdaExample {
    public static void main(String[] args) {
        // Lambda expression implementing MyLambda
        MyLambda myLambda = (message) -> System.out.println("Hello, " + message);

        // Using the lambda
        myLambda.myFunction("World");
    }
}
```
In this example, we've created a lambda expression (message) -> System.out.println("Hello, " + message) that implements the myFunction method of the MyLambda functional interface. We then use this lambda to print a greeting using the variable it stored in.

#### Explanation:
A functional interface is an interface with a single abstract method. In this case, MyLambda serves as our functional interface.

The lambda expression (message) -> System.out.println("Hello, " + message) is a concise way of representing an instance of the functional interface. It takes a String parameter named message and prints a greeting.

When the Java code is compiled, the compiler translates the lambda expression into a class file. Behind the scenes, the compiler generates a synthetic class that implements the functional interface. This class contains the logic specified in the lambda expression.

Java's type inference allows the compiler to determine the type of the lambda expression's parameter. In this case, it infers that the message parameter is of type String.
Functional Programming Paradigm:

The lambda expression embraces the functional programming paradigm, treating functions as first-class citizens. This enables writing more concise and expressive code.

The lambda expression (message) -> System.out.println("Hello, " + message) is essentially syntactic sugar for creating an anonymous class instance implementing the MyLambda interface. The compiler transforms it into something similar to:

```java
class LambdaExample$1 implements MyLambda {
    @Override
    public void myFunction(String message) {
        System.out.println("Hello, " + message);
    }
}
```
This anonymous class is then instantiated and used as if you had created a separate class explicitly.

In summary, lambdas in Java provide a more readable and expressive way to represent functional interfaces. The compiler plays a crucial role in transforming these lambda expressions into the corresponding bytecode, allowing for the seamless integration of functional programming concepts in Java.


### More Examples the merrier !!
```java
import java.util.ArrayList;
import java.util.List;

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```
Now, let's create a list of Person objects and sort them based on their age using a lambda expression:

```java
import java.util.Collections;
import java.util.List;

public class LambdaSortingExample {
    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();
        people.add(new Person("Alice", 25));
        people.add(new Person("Bob", 30));
        people.add(new Person("Charlie", 22));

        // Sorting the list based on age using lambda expression
        Collections.sort(people, (person1, person2) -> person1.getAge() - person2.getAge());

        // Displaying the sorted list
        System.out.println("Sorted People List:");
        for (Person person : people) {
            System.out.println(person);
        }
    }
}
```
In this example, the Collections.sort method is used to sort the list of Person objects based on their age. The lambda expression (person1, person2) -> person1.getAge() - person2.getAge() defines the comparison logic for sorting.

The lambda expression takes two Person objects, person1 and person2, and compares their ages using the getAge method. The result of the comparison determines the order of the objects in the sorted list.

When you run this program, it will display the list of people sorted by age:
```s
Sorted People List:
Person{name='Charlie', age=22}
Person{name='Alice', age=25}
Person{name='Bob', age=30}
```
This demonstrates how you can use lambda expressions to customize the sorting behavior for a list of custom objects.

### Summary:
This post provides a comprehensive overview of Java lambda expressions, covering their importance, syntax, usage, and examples. It starts by explaining the significance of lambda expressions in Java, emphasizing their role in simplifying code for functional interfaces, iteration, comparison, and defining simple tasks. The post highlights how Java adopted lambda expressions, drawing inspiration from JavaScript's functional programming concepts.

---