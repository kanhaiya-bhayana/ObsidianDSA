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