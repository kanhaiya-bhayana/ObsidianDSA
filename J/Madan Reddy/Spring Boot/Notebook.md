## What is Bean?

In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans.
## What is Maven?

Maven is an open-source build tool that helps manage projects and dependencies for Java applications.

## What is GroupId & ArtifactId?

**GroupId** is the organizationId like `com.google`

**ArtifactId** is the project name like `accountdata`


## Want to change the path of .m2 for local repository?

>Change the settings file
>Path: `<maven-installed>/conf/setting.xml`
>Search for the local Repository and play there


## What is @Configuration annotation?

In Spring Boot, the @Configuration annotation indicates that a class is a source of bean definitions.

## What is @Component annotation?

@Component is one of the most commonly used stereotype annotation used by developers. Using this we can easily create and add a bean to the Spring context by writing less code compared to the @Bean option. With stereotype annotations, we need to add the annotation above the class for which we need to have an instance in the Spring context.

## What is difference between @component and @ComponetScan in java spring boot?

The `@Component` annotation is used to declare a class as a Spring-managed component, while `@ComponentScan` is used to define the scope of the component scanning process. In Spring Boot, `@ComponentScan` is implicitly included in `@SpringBootApplication`, scanning the package of the main class and its sub-packages by default.

## @Service

This annotation is used to indicate that a class represents a service component in the application. It is typically used to annotate classes that contain business logic.

## @Repository

It is used to mark the class as DAO. Used on class that has database persistent logic.

## @RestController

Mark class as REST controller. It is a specialized version of the @Controller annotation that includes the @ResponseBody annotation by default.

## @RequestMapping

Used to map specific **url** to **method**. Used on class as well as method level.

```json
@RequestMapping(path = "/api", produces = {MediaType.APPLICATION_JSON_VALUE})
```
## Difference between @Bean and @Component?

### **Key Differences**

|Aspect|`@Bean`|`@Component`|
|---|---|---|
|**Where Used**|Inside a method of a configuration class (`@Configuration`).|Directly on a class.|
|**Discovery Mechanism**|Explicitly declared and returned by a method.|Automatically discovered via component scanning.|
|**Use Case**|Used when you need fine-grained control over bean instantiation (e.g., third-party classes).|Used for application classes you write yourself that Spring can manage.|
|**Customization**|Full control over how the bean is created, including parameters, initialization, etc.|Follows default instantiation via the class constructor.|
|**Dependencies**|Can pass custom parameters during instantiation.|Dependencies are usually injected via `@Autowired`.|


## Spring Boot Starter Parent

The _spring-boot-starter-parent_ project is a special starter project that provides default configurations for our application and a complete dependency tree to quickly build our _Spring Boot_ project. It also provides default configurations for Maven plugins, such as _maven-failsafe-plugin_, _maven-jar-plugin_, _maven-surefire-plugin_, and _maven-war-plugin_.

Beyond that, it also inherits dependency management from _spring-boot-dependencies,_ which is the parent to the s_pring-boot-starter-parent_.

We can start using it in our project by adding it as a parent in our project’s _pom.xml_:

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.1.5</version>
</parent>
```

We can always get the latest version of [_spring-boot-starter-parent_](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-parent) from Maven Central.

## PostConstruct 

The **@PostConstruct** annotation is used on a method that needs to be executed after dependency injection is done to perform any initialization.
## @PreDestroy

The  **@PreDestroy** annotation is used on a method as a callback notification to signal that the instance is in the process of being removed by the container.

## Bean Factory | XMLBeanFactory

- This is the root interface for accessing a Spring bean container.
- It is the actual container that instantiates, configures, and manages a number of beans.

## Application Context
ApplicationContext is a central interface to the Spring Framework that provides the following key functionalities:
1. Bean Management
2. Dependency Injection
3. BeanFactory Extension

ApplicationContext represents the Spring IoC container that holds all the beans created by the application. It is responsible for instantiating, configuring, and creating the beans. Additionally, it gets the beans’ information from configuration metadata provided in XML or Java.
ApplicationContext represents the sub-interface of BeanFactory. In addition to the functionalities of BeanFactory.

Each bean is instantiated after the container has been started since it uses eager loading.

## What are the different modules in spring?
Spring has seven core modules:
1. The Core container module
2. Application context module
3. AOP (Aspect Oriented Programming)
4. JDBC abstraction and DAO module
5. ORM module (Object/Relational)
6. Web Module
7. Test

## @Autowired vs @Inject 

You can annotate fields and constructor using @Autowired to tell Spring framework to find dependencies for you. The @Inject annotation also serves the same purpose, but the main difference between them is that @Inject is a standard annotation for dependency injection and @Autowired is spring specific.

## @Bean vs @Component
**@Bean annotation** returns an object that sring should register as bean in application context. The body of the method bears the logic responsible for creating the instance.
**@Component** preferable for component scanning and automatic wiring.

## What are the different bean scopes in spring?
1. **Singleton**: The bean instance will be only once and same instance will be returned by the IOC container. It is the default scope.
2. **Prototype**: The bean instance will be created each time when requested.
3. **Request**: The bean instance will be created per HTTP request.
4. **Session**: The bean instance will be created per HTTP session.



