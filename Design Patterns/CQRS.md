### Stands for Command Query Responsibility Segregation 

#### Logical and Physical Implementation of CQRS

###### Logical Implementation of  CQRS
- Splitting operations, not Databases. 
- Separate the read (query) operations from the write (command) operations at the code level, but not necessarily at the database level.
- Even though the same database is used, the path for reading and writing data are distinct.

###### Physical Implementation of CQRS
- Separate Databases
- Splitting the read and write operations not just at the code level but also physically using separate databases.
- Introduces data consistency and synchronization problems.

#### Command and Command Handler in CQRS
###### Commands
- Change the state of the system.
- Encapsulates the data required for a specific action.
###### Command Handlers
- Each command has a corresponding handler.
- The handler interacts with the domain model and persists changes.
- Executes business logic and update the state through repositories.



