- Core characteristic of the microservices architecture is the loose coupling of services. every service should have its own databases, it can be polyglot persistence among to microservices.  
- E-commerce application. We will have Product - Ordering and SC microservices that each services data in their own databases. Any changes to one database don't impact other microservices.  
- The service's database can't be accessed directly by other microservices. Each service's persistent data can only be accessed via Rest APIs.
###### Benefits of the Database-per-Service Pattern with Polygot Persistence
- Data schema changes made easy without impacting other microservices.  
- Each database can scale independently.  
- Microservices domain data is encapsulated within the service.  
- If one of the database server is down, this will not affect to other services.  
- Polyglot data persistence gives ability to select the best optimized storage needs per microservices.
###### E-Commerce with Database-per-Service Pattern with Polygot Persistence
- Product service using NoSQL document database for storing catalog related data.  
- Shopping cart service using a distributed cache that supports its simple, key-value data store.  
- Ordering service using a relational database to handle the rich relational structure.