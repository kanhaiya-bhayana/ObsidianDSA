## Element Collection

In Spring Boot (and Hibernate), **element collection** is a feature that allows you to map a collection of basic or embeddable types as part of an entity. This is useful when you want to associate a single entity with a collection of non-entity values (e.g., Strings, Integers, or Embeddable objects) without creating a separate entity class.

### Key Features of Element Collection

1. **Stored in a Separate Table**: The collection is typically stored in a separate table from the entity owning it.
2. **Basic or Embeddable Types**: It supports basic types (e.g., `String`, `Integer`) or embeddable types (annotated with `@Embeddable`).
3. **No Identity**: The elements in the collection don’t have a separate identity like entities do.

### How to Use Element Collection

#### Example with Basic Types

```java
import jakarta.persistence.*;
import java.util.List;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ElementCollection
    @CollectionTable(name = "user_emails", joinColumns = @JoinColumn(name = "user_id"))
    @Column(name = "email")
    private List<String> emails;

    // Getters and setters
}
```

#### Explanation

1. **`@ElementCollection`**: Specifies that this field is an element collection.
2. **`@CollectionTable`**: Defines the table to store the collection (`user_emails` in this case).
3. **`@JoinColumn`**: Specifies the foreign key column (`user_id`) in the collection table.
4. **`@Column`**: Maps the individual elements in the collection.

#### Database Table Structure

For the above example:

* The `User` entity is stored in a `user` table.
* The `emails` list is stored in a separate `user_emails` table, which has:

  * A `user_id` column to link to the `user` table.
  * An `email` column to store the list elements.

#### Example with Embeddable Types

You can also use `@Embeddable` to create a custom type:

```java
@Embeddable
public class Address {
    private String street;
    private String city;

    // Getters and setters
}

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ElementCollection
    @CollectionTable(name = "user_addresses", joinColumns = @JoinColumn(name = "user_id"))
    private List<Address> addresses;

    // Getters and setters
}
```

#### Benefits

1. Easy mapping of non-entity data types.
2. Simplifies relationships when entities are unnecessary.
3. Reduces boilerplate code by eliminating the need for separate entity classes.

#### Limitations

1. Lack of identity for elements: You cannot query individual elements directly.
2. Less flexibility: Changes to the collection often result in updates to the entire collection.

This feature is great for simple, structured data associated with an entity but not requiring a separate entity representation.
