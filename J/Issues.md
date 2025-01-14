#### Getting the errors like cannot do get on entities and used lombok 

- Fix: Try to downgrade the version of spring boot and keep the below plugins in the build | Till 10-01-2025 the below version is working:

```xml
<parent>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-parent</artifactId>  
    <version>3.3.5</version>  
    <relativePath/> <!-- lookup parent from repository -->  
</parent>

<build>  
    <plugins>  
       <plugin>  
          <groupId>org.springframework.boot</groupId>  
          <artifactId>spring-boot-maven-plugin</artifactId>  
          <configuration>  
             <excludes>  
                <exclude>  
                   <groupId>org.projectlombok</groupId>  
                   <artifactId>lombok</artifactId>  
                </exclude>  
             </excludes>  
          </configuration>  
       </plugin>  
    </plugins>  
</build>
```


#### Jwt - dependencies

```xml
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-api</artifactId>  
    <version>0.11.5</version>  
</dependency>  
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-impl</artifactId>  
    <version>0.11.5</version>  
    <scope>runtime</scope>  
</dependency>  
<dependency>  
    <groupId>io.jsonwebtoken</groupId>  
    <artifactId>jjwt-jackson</artifactId>  
    <version>0.11.5</version>  
    <scope>runtime</scope>  
</dependency>
```