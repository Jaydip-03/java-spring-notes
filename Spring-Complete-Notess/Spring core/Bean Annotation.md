> [!TIP]
> Use `@Bean` when you need explicit control over how Spring creates and registers an object.

| Topic | Summary |
|---|---|
| Target | A method whose returned object should be registered as a bean |
| Common location | A `@Configuration` class |
| Typical use | Explicit object creation, including third-party classes |

## @Bean

- `@Bean` is a Spring annotation used on a method to register its returned object as a bean in the Spring IoC Container. It is commonly used inside `@Configuration` classes and is especially useful when we need explicit control over object creation, including objects from third-party libraries.

- It is a method level annotation
- Whenever using this annotation object creation done by programmer and that object will be managed by spring container
- It is recommended to used when we need object of predefined class Because we cannot used component annotation on top of predefined classes
- If we want, we can use this annotation for custom classes, but it not recommended
- Whenever we need object of predefined classes inside spring project and if we want spring container to manage or inject predefined class object then we will use bean annotation
