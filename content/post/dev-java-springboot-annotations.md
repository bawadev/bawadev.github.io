---
layout:     post
title:      "Introduction to Docker"
subtitle:   "A zero to hero docker guide for java developers"
date:       2024-01-09
author:     "Pasindu"
image:      "https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fc4kzfzbh6wi2ukkodudl.png"
summary: "Spring Boot is a powerful framework that simplifies the development of production-ready applications. Annotations play a crucial role in Spring Boot by providing metadata and configuration to the Spring container. This guide explains various essential annotations used in Spring Boot, their usage, and provides examples for each."
tags:
    - Java
    - Spring boot
    - Spring annotations  
---

Spring Boot is a powerful framework that simplifies the development of production-ready applications. Annotations play a crucial role in Spring Boot by providing metadata and configuration to the Spring container. This guide explains various essential annotations used in Spring Boot, their usage, and provides examples for each. 

## Table of Contents

1. [@SpringBootApplication](#springbootapplication)
2. [@Autowired](#autowired)
3. [@RestController](#restcontroller)
4. [@Entity](#entity)
5. [@Id](#id)
6. [@GeneratedValue](#generatedvalue)
7. [@Service](#service)
8. [@Repository](#repository)
9. [Validation Annotations](#validation-annotations)
10. [Lombok Annotations](#lombok-annotations)
11. [Custom Exception Handling](#custom-exception-handling)

## @SpringBootApplication

### Explanation

- **@Target(ElementType.TYPE)**: This meta-annotation indicates that @SpringBootApplication can only be used on classes.
- **@Retention(RetentionPolicy.RUNTIME)**: Specifies that the annotation is available at runtime.
- **@Inherited**: Allows subclasses to inherit the annotation.
- **@EnableAutoConfiguration**: Enables automatic configuration of beans based on the classpath settings.
- **@SpringBootConfiguration**: Indicates that the class is a Spring Boot configuration class, typically used as the main configuration class.
- **@ComponentScan**: Enables component scanning to automatically detect and register beans within the specified package.

### Usage

Used to mark the main class of a Spring Boot application, aggregating several other annotations.

### Example

```java
@SpringBootApplication
public class MySpringBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class, args);
    }
}
```

## @Autowired

### Explanation

- Automatically wires beans to satisfy dependencies.

### Usage

Used to inject dependencies automatically.

### Example

```java
@Service
public class MyService {
    private final MyRepository myRepository;

    @Autowired
    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }
}
```

## @RestController

### Explanation

- **description**: Indicates that the class is a controller where each method returns a domain object instead of a view. It is a specialized version of @Controller.

### Usage

Used to create RESTful web services.

### Example

```java
@RestController
@RequestMapping("/api")
public class MyController {
    
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, World!";
    }
    
    @PostMapping("/create")
    public MyEntity createEntity(@RequestBody MyEntity entity) {
        // create and return entity
    }
    
    @DeleteMapping("/delete/{id}")
    public void deleteEntity(@PathVariable("id") Long id) {
        // delete entity by id
    }
    
    @PutMapping("/update/{id}")
    public MyEntity updateEntity(@PathVariable("id") Long id, @RequestBody MyEntity entity) {
        // update and return entity
    }
}
```

## @Entity

### Explanation

- Marks a class as a JPA entity representing a table in the database.

### Usage

Used to define a persistent domain object.

### Example

```java
@Entity
public class MyEntity {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    
    private String name;
    
    // getters and setters
}
```

## @Id

### Explanation

- Marks a field as the primary key of the entity.

### Usage

Used in conjunction with @Entity to define the primary key.

### Example

```java
@Entity
public class MyEntity {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    
    private String name;
    
    // getters and setters
}
```

## @GeneratedValue

### Explanation

- Configures the way of incrementing the specified field (usually a primary key).

### Usage

Used with @Id to specify the generation strategy for the primary key.

### Example

```java
@Entity
public class MyEntity {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    
    private String name;
    
    // getters and setters
}
```

## @Service

### Explanation

- Indicates that the annotated class is a service component in the service layer.

### Usage

Used to define a service that contains business logic.

### Example

```java
@Service
public class MyService {
    
    public String performService() {
        return "Service performed";
    }
}
```

## @Repository

### Explanation

- Indicates that the annotated interface is a repository, which abstracts data access and is used in the persistence layer.

### Usage

Used to define a data repository.

### Example

```java
@Repository
public interface MyRepository extends JpaRepository<MyEntity, Long> {
    // custom query methods
}
```

## Validation Annotations

### Explanation

- **spring-boot-starter-validation**: Provides validation support.
- **@NotBlank**: Validates that a field is not null and the trimmed length is greater than zero.
- **@Valid**: Triggers validation on the annotated element.

### Usage

Used for validating entity fields.

### Example

```java
@Entity
public class MyEntity {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    
    @NotBlank(message = "Name cannot be blank")
    private String name;
    
    // getters and setters
}
```

In a controller:

```java
@PostMapping("/create")
public ResponseEntity<MyEntity> createEntity(@Valid @RequestBody MyEntity entity) {
    // handle the creation
}
```

## Lombok Annotations

### Explanation

- **@Data**: Generates getters, setters, equals, hashCode, and toString methods.
- **@Builder**: Implements the builder pattern.

### Usage

Used to reduce boilerplate code.

### Example

```java
@Data
@Builder
@Entity
public class MyEntity {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    
    @NotBlank(message = "Name cannot be blank")
    private String name;
}
```

## Custom Exception Handling

### Explanation

- **@ControllerAdvice**: Allows global exception handling. Annotated class must extend `ResponseEntityExceptionHandler`.
- **@ResponseStatus**: Specifies the HTTP status code to return.

### Usage

Used for handling exceptions globally.

### Example

Custom exception:

```java
public class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}
```

Controller advice:

```java
@ControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(CustomException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ResponseEntity<String> handleCustomException(CustomException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```

---

This guide provides an extensive overview of various annotations used in Spring Boot, covering their explanations, usage, and practical examples. By understanding and using these annotations effectively, you can streamline the development process and enhance the maintainability of your Spring Boot applications.