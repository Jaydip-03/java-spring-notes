## Introduction

Dependency Injection (DI) is one of the core concepts in the Spring Framework, playing a crucial role in making applications modular, testable, and easy to maintain. To understand the significance of DI, it's essential to first look at how developers traditionally managed dependencies and the challenges they faced.

## The Traditional Approach: Manual Dependency Management

**How Dependencies Were Traditionally Managed?** In traditional object-oriented programming, when one class (Class Demo) needs to use another class (Class Play), the developer would create an instance of Class play directly inside Class Demo. This process is called **manual dependency management**. Here’s an example:

public class Demo {
    private Play p;

    public Demo() {
        this.p = new Play();
    }

    public void doSomething() {
        p.performTask();
    }
}

In this example, Class Demo creates an instance of Class Play using the `new` keyword. While this approach works for simple applications, it can lead to several problems as the application grows in complexity.

## Problems with Manual Dependency Management

- **Tight Coupling**: Class A is tightly coupled to Class B. If you want to change the implementation of Class B or replace it with a different class, you have to modify Class A’s code. This makes the code less flexible and harder to maintain.
- **Difficulty in Testing**: Tight coupling makes unit testing difficult. To test Class A, you also need to deal with Class B, which might not be desirable. For example, if Class B connects to a database, you might not want to involve the database during testing.
- **Reduced Reusability**: Because Class A is tightly bound to a specific implementation of Class B, it becomes harder to reuse Class A with different implementations of Class B.
- **Complex Object Graphs**: In larger applications, the number of dependencies can grow, leading to complex interconnections between objects. Manually managing these dependencies becomes increasingly error-prone and challenging.

## The Need for Dependency Injection

**As applications grew in size and complexity, developers needed a way to manage dependencies more efficiently. They needed a method that would allow f**or:

- **Loose Coupling**: Reducing the dependency between classes, making the code more flexible and easier to change.
- **Easier Testing**: Allowing classes to be tested in isolation without requiring their dependencies to be instantiated manually.
- **Better Reusability**: Enabling classes to work with different implementations of their dependencies without needing changes.
- **Simplified Object Management**: Automatically handling the creation and management of objects, especially in complex systems with many interdependent components.

This is where Dependency Injection comes in.


## What is Dependency Injection?

**Dependency Injection (DI)** is a design pattern in which an object’s dependencies are provided to it from an external source, rather than the object creating them itself. In other words, instead of a class being responsible for instantiating its dependencies, those dependencies are injected into the class by an external entity, such as a framework or container. In the Spring Framework, DI is at the core of the IoC (Inversion of Control) principle, where the control of object creation and management is inverted from the application code to the Spring IoC container

It is processes injecting one class object into another class by spring container is known as dependency injection
By using dependency injection, we can inject dependency object into dependent object
Object which is depending on another object to perform its operation is known as dependent object
Object which is used by another r object is known as dependency object
Dependency injection will help us to make spring application as loosely coupled application because in spring application objects are not injected by programmer, it is injected by spring container
We can achieve dependency injection using xml configuration or annotation-based configuration
If you want to achieve dependency injection using annotation, then we will use @Autowired annotation
We can achieve dependency injection mainly in three ways those are
1.
Variable level
2.
Constructor level
3.
Setter level dependency injection


# Variable level dependency injection:

It is processes of injecting one class object into another class by spring container through variable is known as variable level dependency injection

`import org.springframework.stereotype.Component;`  
  
`@Component`  
`public class Simcard {`  
    `public void insert() {`  
        `System.out.println("Sim card inserdted ") ;`  
    `}}` 


`import org.springframework.stereotype.Component;`  
  
`@Component`  
`public class Mobile {`  
  
    @Autowired`  
    private Simcard simcard ;  
  
     public void call() {  
        simcard.insert() ;  
        System.out.println("U can call from Mobile") ;  
    }}


Disadvantages of variable level dependency injection:

Whenever using this dependency injection then we cannot write any validation logic before achieving dependency injection
It is not recommended to use in real time project, but it is used only for learning purpose
To achieve dependency injection spring container will call private variables outside the class and it will break one of the important oops principal that is encapsulation
To access private variable outside the class spring container internally usages reflection concepts
By using reflection concepts, we can access private members outside the class


# Constructor level dependency injection:

It is processes of injecting one class object into another class by spring container through constructor is known as constructor level dependency injection
When bean classis having argument and no argument constructor then it is mandatory to use @Autowired annotation otherwise spring container usages no argument constructor to create object of bean class, but dependency injection logic is present inside argument constructor
If you want to tell spring container to used argument constructor instead of no argument constructor, then we will use @Autowired annotation then it will use parameterized constructor
When bean class are having only one parameterized constructor then it is optional to use @Autowired annotation
When one class is highly dependent on another class then it is recommended to use constructor level dependency inject


`import org.springframework.stereotype.Component;`  
  
`@Component`  
`public class Pen {`  
    `public void write(String type) {`  
        `System.out.println("U R Writing with " + type) ;`  
    `}}`


`@SpringBootApplication`  
`public class DepemdencyInjectionApplication implements CommandLineRunner {`  
  
     `private Pen pen ;`
    
     `public DepemdencyInjectionApplication(Pen pen) {`  
        `this.pen = pen ;`  
   `}`


# Setter method di:

It is processes of injecting one class object into another class through setter method is known as setter level dependency injection
Whenever generating setter method to achieve dependency injection then we will use @Autowired annotation on top of setter method
It we are using this annotation on setter method then internally spring container will call this method to achieve dependency injection
When one class is not highly dependent on another class then it is recommended to use setter level method dependency injection

`@Component`  
`public class Laptop {`  
  
    `private Charger charger ;`  
  
    `@Autowired  // setter method level Di`  
    `public void setCharger(Charger charger) {`  
        `this.charger = charger ;`  
    `}



## Benefits of Dependency Injection

### **1. Loose Coupling**

With DI, classes are not responsible for creating their dependencies. This leads to loose coupling, making the code more modular and easier to maintain.

### **2. Improved Testability**

DI allows for easier testing because dependencies can be easily mocked or replaced with stubs during testing, without changing the code of the class being tested.

### **3. Enhanced Flexibility**

Because dependencies are injected externally, it’s easy to swap out implementations. For example, you can inject a different implementation of a service or repository without changing the class that uses it.

### **4. Simplified Object Management**

The Spring IoC container takes care of creating and managing the lifecycle of beans (objects), reducing the complexity of managing object graphs in large applications.

In this article, we explored the concept of Dependency Injection (DI) and its importance in software development, particularly within the Spring Framework. We began by discussing the traditional approach to dependency management and the challenges it posed, such as tight coupling, difficulties in testing, and reduced reusability. Then, we introduced DI as a solution to these problems, highlighting its ability to create loosely coupled, flexible, and testable code. Finally, we examined how DI is implemented in Spring through constructor, setter, and field injection, along with the benefits it offers in terms of improved testability and simplified object management.