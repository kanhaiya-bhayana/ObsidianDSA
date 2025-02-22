- It is a behavioral design pattern.
- In this when subject changes the state of all its dependent objects notified the changes.
##### OR

The **Observer Design Pattern** is a behavioral design pattern used to define a one-to-many dependency between objects. When one object (called the **Subject**) changes its state, all its dependent objects (called **Observers**) are notified and updated automatically. This pattern promotes a loose coupling between the Subject and its Observers.

### Key Concepts

1. **Subject**: The core entity whose state changes need to be observed.
2. **Observers**: Entities that depend on the state of the Subject and react to its changes.
3. **Loose Coupling**: The Subject doesn't need to know the concrete implementation details of its Observers; it only knows that they follow a common interface.

[Observer - Repos](https://dev.azure.com/ikanhaiyabhayana/Dev/_git/LLD?path=/Observer)
