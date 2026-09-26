## Introduction to Spring Core

**Spring Core** is the fundamental part of the **Spring Framework** that provides the core features required to build Spring applications.

The main purpose of Spring Core is to provide **IoC (Inversion of Control)** and **Dependency Injection (DI)**, which help developers create **loosely coupled, maintainable, and testable applications**.

Spring Core manages the creation, configuration, and lifecycle of objects through the **Spring Container**.

### Main concepts of Spring Core

Spring Core mainly revolves around:

- **IoC (Inversion of Control)**
- **Dependency Injection (DI)**
- **Spring Container**
- **Beans**
- **BeanFactory**
- **ApplicationContext**
- **Bean Configuration**
- **Bean Lifecycle**
- **Bean Scopes**

# Basic working
In a traditional Java application, developers generally create and manage objects themselves:

Developer
   ↓
Creates Object
   ↓
Manages Dependencies
   ↓
Uses Object

# With Spring Core:

              Spring Container
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
     Creates Objects       Injects Dependencies
          │                     │
          └──────────┬──────────┘
                     ↓
                Application



---------------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------

# Inversion of Control (IoC)

## What is IoC?

Inversion of Control (IoC) is one of the core principles of the Spring Framework, and it fundamentally changes the way objects are created and managed in an application. It shifts the responsibility of managing object lifecycles from the application code to the **Spring IoC container**, leading to more modular, testable, and maintainable applications.

## What IOC container does?

The **IoC Container** is responsible for creating objects, injecting dependencies, and managing the entire lifecycle of these objects. This process, known as Dependency Injection (DI), decouples the application's components, allowing for more flexible and maintainable code

With IoC, this responsibility is transferred to an external **container/framework**.
In Spring : The Spring IoC Container takes responsibility for creating, configuring, managing, and connecting application objects (Beans)

## Why the term “Inversion of Control”?

In a traditional application, developers have to manually manage object creation and their dependencies, which can lead to tightly coupled code that's hard to maintain and scale. Spring IoC inverts this control by handing over the responsibility of object creation and dependency management to the framework itself. This is where the term "Inversion of Control" comes from—developers no longer control the flow of their application directly; instead, they rely on the IoC Container to manage the application's components.

## From where does the IOC container gets the information of objects?

The IoC Container uses various configuration methods XML, Java annotations, and Java code to understand the objects that make up the application. These objects, known as **Beans**, are then managed by the container. Whether it's creating a new instance of a class, setting its properties, or handling its destruction, the IoC Container takes care of everything, freeing developers to focus on the application's business logic

--------------------------------------------------------------------
# First understand "Control"

Let's take a simple example.

`public class Car {`

    `Engine engine = new Engine();`

`}`

Here, `Car` controls the creation of `Engine`
Car
 │
 │ creates
 ↓
Engine


What happens with IoC ?
With IoC, `Car` doesn't create its `Engine`.
Instead. 

public class Car {

    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }
}

Now `Car` says:  "I need an Engine."
But it doesn't say: "I will create the Engine."

Someone else provides it.
In Spring, that "someone else" is the **IoC Container**.

             Spring IoC Container
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
      Engine Object          Car Object
                                  │
                                  │ receives
                                  ↓
                               Engine



---------------------------------------------------------------------------------------------------------------------------------------

# What is the Spring Container?

The Spring Container is the core component of the Spring Framework that manages the lifecycle of Spring Beans. It is responsible for creating, configuring, and managing beans and resolving their dependencies through Dependency Injection. The two major container interfaces are BeanFactory and ApplicationContext, with ApplicationContext being commonly used in modern Spring applications.

If any class object is created and managed by Spring container then that class is known as Bean class
Spring container is responsible to inject one class object into another class object
Spring container is responsible to destroy bean class object
Form object creation to object destruction Spring container is responsible to manage Bean class object
Spring container is responsible to manage Bean Life cycle
Spring containers make spring applications as loosely coupled
Spring container usages singleton design patterns to create object of Bean class


----------------------------------------------------------------------------------------------------------------------------------------

**BeanFactory**

This is the most basic type of IoC container in Spring. It provides the essential features needed to manage objects (called Beans) in your application. BeanFactory is lightweight and perfect for simple applications where you only need basic dependency injection.
If you want to create object of bean class core container then it will take help of bean factory

Bean factory is a interface
It is provided by spring framework
If you want to create of object of bean factory then it will take help of child implementing class that is xml bean factory
Public XMLFactory(Resource resource){
}
We will use above constructor to create object of bean factory
Whenever using this constructor we have to pass resource object as a argument
Resource is an interface
It is provided by spring framework
We will take the help of child implementing class of resource interface that is class path resources
Public classPathResource(String configFileName){
}

Whenever above using constructor to create object of classPathResources then we need to pass configuration file name with extensions in the form of String
−
Eg

```
public class App {

    public static void main(String[] args) {

        ClassPathResource resource =
                new ClassPathResource("beans.xml");

        BeanFactory factory =
                new XmlBeanFactory(resource);

        Engine engine =
                factory.getBean("engine", Engine.class);

        engine.start();
    }
}
```


Limitations of BeanFactory:

Whenever using BeanFactory then we are writing deprecated code(XmlBeanFactory)
BeanFactory will not support to annotation
Whenever using BeanFactory then we need to use xml file as a configuration file but in the current IT market xml-based configuration outdid
BeanFactory is a lazy instantization because it will not create bean class object until we call getBean() method
To overcome all these limitations, we will use application context

##  GetBean(BeanclassName.class):
It is abstract method
It is present in beanFactory
It will take bean class as a argument in the form of .class
It will help us to get the object which are created by spring container
It return type is generic (bean class)
Whenever using this method always we need to pass bean class as a argument
If we are passing any other class then it will throw exception

----------------------------------------------------------------------------------------------------------------------------------------


## Application Context

This is a more advanced type of IoC container that extends the capabilities of BeanFactory. In addition to the basic features, ApplicationContext offers more robust options like event propagation, declarative mechanisms to create a Bean, and a more extensive lifecycle management. It's typically the go-to choice for most Spring applications because of its powerful features.

If we want j2ee container to create object of bean class, then we will take the help of application context
Application context is a early instantiation it means it will create all bean class object during classloading processes
Application context will support xml-based configuration as well as annotation based configuration
Application context is used by default inside spring boot framework to create object of bean class
To create object of application context we will take the help of child implementing class that AnnotationConfigApplicationContext(config.class)
Public AnnotationConfighApplicationContext(AnnotationConfigurationclass.class){
}
Whenever using above constructor, we need to pass configuration class name in the form of .class
It is a child of BeanFactory
It is recommended to use whenever using spring framework

`public class Engine {
    public void start() {
        System.out.println("Engine started");
    }
}

public class Car {
    private Engine engine;

    // Constructor Injection
    public Car(Engine engine) {
        this.engine = engine;
    }

    public void drive() {
        engine.start();
        System.out.println("Car is driving");
    }
}

// Configuration using Java Annotations
@Configuration
public class AppConfig {

    @Bean
    public Engine engine() {
        return new Engine();
    }

    @Bean
    public Car car() {
        return new Car(engine());
    }
}

// Running the Spring Application
public class SpringIoCExample {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        Car car = context.getBean(Car.class);
        car.drive();
    }
}`


![[Pasted image 20260924150234.png]]


## @Component  Annotation

It is given by spring framework
It is present in org.springframework.stereotype
It is stereotype annotation
It will help us to make class as a bean class
If are using this annotation to any class then the class object will be created and managed by Spring container

`@Component`
  `public class Phone {`

    public Phone() {
    System.out.println("phone object is created");
    }

   `}



@Configuration:
It will help us to make class as configuration
Spring container will always search for configuration class
Inside configuration class we can do bean class configuration, we can defined based package of bean class ....etc

@ComponentScan:
It is class level annotation
It will help us to define based packages
It will help us to tell spring container which package has to be scan to getBean class
Spring container we scan only those packages which are defined inside component scan annotation
If we are not defining any packages, then component scan will take configuration class package name as a based Package
Inside componentScan package we can define more than one packages
Eg

`@Configuration`
 `@ComponentScan`
 `public class Config {`
 `}`



==------------------------------------------------------------------------------------------------

# Spring Context

## What is Spring Context?

The **Spring Context** is the core component of the Spring Framework, representing the Spring IoC (Inversion of Control) container. It is responsible for managing the lifecycle of beans, including their creation, configuration, and destruction. The Spring Context acts as a container that holds the beans and provides them to the application whenever required.

In simple terms, it manages the dependencies between objects, ensuring that they are appropriately initialized and configured before being used.

### **Core Concepts of Spring Context**

Before diving into examples, let's explore some core concepts:

- **Beans**: Objects managed by the Spring IoC container. Beans are instantiated, configured, and assembled by the container.
- **Dependency Injection (DI)**: A technique where the Spring IoC container injects dependencies (beans) into other beans. This decouples the objects, making them easier to manage and test.
- **BeanFactory**: The most basic container in Spring that provides the configuration framework and basic functionality. However, it is often replaced by `ApplicationContext` in modern applications.
- **ApplicationContext**: A more advanced container that provides additional features like event propagation, declarative mechanisms to create a bean, and integration with Spring's AOP.

## Types of ApplicationContext

Spring provides several implementations of the `ApplicationContext`, each suitable for different use cases:

- **ClassPathXmlApplicationContext**: Loads the context definition from an XML file located in the classpath.
- **FileSystemXmlApplicationContext**: Loads the context definition from an XML file in the file system.
- **AnnotationConfigApplicationContext**: Loads the context definition from Java-based configuration classes using annotations.
- **WebApplicationContext**: A specialized version of `ApplicationContext` used in web applications.

### Example: Setting Up Spring Context with Annotations

`package com.jay.springcontext;  
  
import org.springframework.stereotype.Component;  
  
`@Component`  
`public class Greeting {`  
    `public void greet() {`  
        `System.out.println("Hello from greetingclass") ;`  
    `}`
    `}`


`package com.jay.springcontext;`  
  
`import org.springframework.boot.CommandLineRunner;`  
`import org.springframework.boot.SpringApplication;`  
`import org.springframework.boot.autoconfigure.SpringBootApplication;`  
`import org.springframework.context.ApplicationContext;`  
`import org.springframework.context.ConfigurableApplicationContext;`  
`import org.springframework.context.annotation.AnnotationConfigApplicationContext;`  
 `@SpringBootApplication`  
`public class SpringContextApplication {`  
  
     public static void main(String[] args) {  
        ConfigurableApplicationContext cac =  
        SpringApplication.run(SpringContextApplication.class, args); 
  
        Greeting g =  cac.getBean(Greeting.class) ;  
         g.greet() ;  
  
          
    }
      
  
`}`