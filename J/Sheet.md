##### Do not want to fetch the password from the database
- Use the below annotation, It will ignore the password field

```java
@JsonProperty(access = JsonProperty.Access.WRITE_ONLY)  
private String password;
```

###### Example of User Entity

```java
@Entity  
@Data  
public class User {  
    @Id  
    @GeneratedValue(strategy = GenerationType.AUTO)  
    private UUID id;  
  
    private String fullName;  
    private String email;  
  
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)  
    private String password;  
}
```


