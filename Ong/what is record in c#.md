<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# what is record in c\#

A **record** in C\# is a special type of class or struct designed primarily for encapsulating data, with built-in functionality that makes it ideal for immutable data models and value-based equality comparisons[^1][^5][^6].

## Key Features of Records in C\#

**1. Concise Syntax for Data Models**

- Records allow you to define data-centric types with less boilerplate code compared to regular classes or structs.
- Example:

```csharp
public record Person(string FirstName, string LastName);
```

This creates an immutable type with two properties and automatically implements equality, printing, and cloning behaviors[^1][^3].

**2. Immutability by Default**

- Record classes are immutable by default, meaning their properties can only be set during object creation (using the `init` accessor)[^2][^3][^5].
- You can use the `with` expression to create a copy of a record with some properties changed:

```csharp
var person2 = person1 with { LastName = "Smith" };
```


**3. Value Equality**

- Unlike regular classes (which use reference equality), records use value equality. Two record instances are considered equal if all their properties are equal, regardless of whether they are the same object in memory[^3][^5][^6].
- This is especially useful for scenarios like data transfer objects (DTOs), configuration, or messages where the content matters more than the identity[^7].

**4. Types of Records**

- **Record Class**: Reference type, immutable by default.

```csharp
public record class Person(string Name);
```

- **Record Struct**: Value type, mutable by default unless marked as `readonly`.

```csharp
public record struct Point(double X, double Y);
```

You can make a record struct immutable by adding the `readonly` modifier[^1][^4].

**5. Compiler-Generated Methods**

- The compiler generates useful methods for records, such as:
    - `Equals` and `GetHashCode` for value equality
    - `ToString` for readable output
    - `Deconstruct` for positional records[^5][^6]


## When to Use Records

- When you need a type that is defined by its data, not its identity.
- When you want immutability and value-based comparison out of the box.
- For data transfer objects, configuration, or any scenario where the primary concern is the data being stored and compared[^5][^7].


## Example Comparison: Class vs Record

| Feature | Class | Record |
| :-- | :-- | :-- |
| Default Equality | Reference equality | Value equality |
| Immutability | Mutable by default | Immutable by default |
| Syntax | More verbose | Concise, especially for DTOs |
| Use Case | Complex behavior, identity | Data-centric, value comparison |

## Summary

A record in C\# is a modern, concise way to define data-centric types that are immutable and support value equality, making them ideal for scenarios where the data itself, rather than the object's identity, is most important[^1][^3][^5].

<div>⁂</div>

[^1]: https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record

[^2]: https://www.c-sharpcorner.com/article/c-sharp-9-0-introduction-to-record-types/

[^3]: https://dev.to/bytehide/creating-and-working-with-records-in-c-47io

[^4]: https://blog.ndepend.com/c-record-explained/

[^5]: https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/records

[^6]: https://code-maze.com/csharp-records/

[^7]: https://www.reddit.com/r/csharp/comments/1cggiu7/can_anybody_explain_records_from_a_practical/

[^8]: https://www.youtube.com/watch?v=M_TQgNKAie8

[^9]: https://ironpdf.com/blog/net-help/csharp-record/

