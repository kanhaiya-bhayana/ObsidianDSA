# 1. Why will you choose Spring Boot over Spring framework?

1. Dependency Resolution/Avoid Version conflict
2. Avoid additional configuration
3. Embeded Tomcat, Jetty (no need to deploy WAR files)
4. Provide production-ready features such as metrics, health checks


# 2. What all spring boot starter you have used or what all module you have worked on?
1. spring boot starter web
2. spring starter data JPA
3. spring boot starter AOP
4. spring boot starter web services
5. spring boot starter security
6. Spring Boot Starter for Apache Kafka
7. Spring Boot Starter for Spring Cloud
8. spring boot starter thymeleaf

https://spring.io/projects/spring-data


# 3. How will you run your Spring Boot Application?

### **Key Differences at a Glance**:

|Aspect|Run Button in IDE|`mvn spring-boot:run`|
|---|---|---|
|**Execution**|Direct invocation of `main()` method.|Maven plugin executes the application.|
|**Dependency Management**|IDE's classpath configuration.|Maven resolves dependencies.|
|**Build Updates**|May not reflect recent `pom.xml` changes.|Reflects recent `pom.xml` changes.|
|**Speed**|Typically faster for development.|Slower due to build and dependency checks.|
|**Debugging**|Easy to attach a debugger automatically.|Requires manual debugging configuration.|
|**Environment Consistency**|Depends on IDE configuration.|Consistent with Maven build lifecycle.|
```sh
mvn spring=boot:run
```
