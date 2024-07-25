- Microservices are small, independent, and loosely coupled services that can work together.
- Each Service is a separate codebase, which can be managed by a small development team.
- Microservices communicate with each other by using well-defined APIs.
- Can be deployed independently and autonomously.
- Can work with many different technology stacks which is technology agnostic.
- Has its own database that is not shared with other service.
###### Architecture
The microservice architecture style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP or GRPC API.
- MS are built around business capabilities and independently deployable by fully automated deployment process.
- MS architecture decomposes an application into small independent services that communicate over well-defined APIs. Service are owned by small, self-contained teams.
- MS architecture is cloud native architectural approach in which services composed of many loosely coupled and independently deployable smaller components.
- MS have their own technology stack, communicate to each other over a combination of REST  APIs, are organized by business capability, with the bounded contexts.
###### Characteristics
- Componentization via Services
- Organized around Business Capabilities
- Products not Projects
- Smart endpoints and dumb pipes
- Decentralized Governance
- Decentralized Data Management
- Infrastructure Automation
- Design for failure
###### Benefits
- Agility, Innovation and Time-to-market
	- MS architectures makes applications easier to scale and faster to develop, enabling innovation and accelerating tome-to-market for new features.
- Flexible Scalability
	- MS can be scaled independently, so you scale out sub-services that require less resources, without scaling out the entire application.
- Small, focused teams
	- MS should be small enough that a single feature team can build, test, and deploy it.
- Small and separated code base
	- MS are not sharing code or data stores with other services, it minimizes dependencies, and that makes easier to adding new features.
###### Challenges of MS Architecture
- Complexity
	- Each service is simpler, but the entire system is more complex. Deployments and Communications can be complicated for hundreds of MSs.
- Network problems and latency
	- MS communicate with inter-service communication, we should manage n/w problems. Chain of services increase latency problems and become chatty API calls.
- Development and Testing
	- Hard to develop and testing these E2E processes in MS arch; if we compare to monolithic ones.
- Data Integrity
	- MS has its own data persistence. Data consistency can be a challenge. Follow eventual consistency where possible.
---
"Chatty API calls" in the context of microservices architecture (MSA) refer to a scenario where services frequently communicate with each other, often with many small, granular requests. This can lead to several issues:

1. **Increased Latency**: Each API call introduces network latency. When services need to make multiple calls to other services to fulfill a single request, the accumulated latency can significantly slow down the overall response time.
2. **Higher Network Overhead**: More frequent API calls result in increased network traffic, which can strain network resources and lead to performance bottlenecks.
3. **Complexity in Error Handling**: With many inter-service calls, the chances of encountering network issues, timeouts, or failures increase. Managing these errors and implementing robust retry mechanisms becomes more complex.
4. **Maintenance Challenges**: The more chatty the inter-service communication, the harder it becomes to maintain and debug the system, as understanding the flow of data and requests through the system requires tracking many small interactions.

To mitigate these issues, it's often recommended to:
- Aggregate data when possible to reduce the number of calls.
- Use asynchronous communication and messaging queues where appropriate.
- Implement caching strategies to avoid repeated calls for the same data.
- Optimize the granularity of services to balance between too coarse and too fine.

In the context of microservices architecture, small granular requests refer to API calls that perform very specific and fine-grained operations. This contrasts with larger, more comprehensive requests that handle multiple operations in a single call. Here's a deeper explanation:

---
###### Small Granular Requests

**Example**: Imagine an e-commerce application with a microservices architecture. The following services might exist:
- **Product Service**: Manages product details.
- **Inventory Service**: Manages stock levels.
- **Pricing Service**: Manages product prices.
- **Order Service**: Manages order processing.

For a single user action, such as displaying product details on a web page, multiple small granular requests might be needed:
1. **Fetch Product Details**: A call to the Product Service to get the product description, images, etc.
2. **Fetch Inventory Status**: A separate call to the Inventory Service to check stock availability.
3. **Fetch Price**: Another call to the Pricing Service to get the current price of the product.
4. **Fetch Reviews**: A call to a Review Service to get user reviews for the product.

###### Characteristics of Small Granular Requests
1. **Specific**: Each request addresses a specific, narrow aspect of functionality.
2. **Frequent**: To fulfill a single user request, multiple API calls are often necessary.
3. **Lightweight**: Each request typically involves a small amount of data and processing.

###### Issues with Small Granular Requests
1. **Increased Latency**: Making multiple calls over the network can introduce delays as each call has its own latency.
2. **Network Overhead**: More frequent calls mean more data packets transmitted over the network, increasing load and potential congestion.
3. **Error Propagation**: With more calls, there's a higher chance that one of them might fail, complicating error handling and retry mechanisms.
4. **Complexity in Coordination**: Managing the sequence and dependencies between these small calls can add complexity to the codebase.

###### Example Scenario: Combining Granular Requests
To reduce the chattiness, microservices might combine multiple granular requests into a single, coarser-grained request. For instance:
- **Aggregated Product Details API**: Instead of making separate calls to the Product, Inventory, Pricing, and Review services, a single API endpoint could be created that aggregates all this data and returns it in one response. This endpoint could internally call the necessary services and consolidate the results before responding.

By aggregating requests, the system can reduce the number of API calls, improve performance, and simplify client-side logic, at the cost of slightly increased complexity in the service that handles aggregation.

---
## When to use MSA
###### Make Sure You Have a "Really Good Reason" for Implementing Microservices 
Check if your application can do without microservices. When your application requires agility to time-to-market with zero-down time deployments and updated independently that needs more flexibility.  
###### Iterate With Small Changes and Keep the Single-Process Monolith as Your "Default" 
Sam Newman and Martin Fowler offers Monolithic-First approach. Single-process monolithic application comes with simple deployment topology. Iterate and refactor with turning a single module from the monolith into a microservices one by one.  
###### Required to Independently Deploy New Functionality with Zero Downtime  
When an organization needs to make a change to functionality and deploy that functionality without affecting rest of the system.  
###### Required to Independently Scale a Portion of Application  
Microservice has its own data persistence. Data consistency can be a challenge. Follow eventual consistency where possible.

## When Not to use MS Anti-Patterns of Microservices
###### Don't do Distributed Monolith  
- Make sure that you decompose your services properly and respecting the decoupling rule like applying bounded context and business capabilities principles. 
- Distributed Monolith is the worst case because you increase complexity of your architecture without getting any benefit of microservices.  
###### Don't do microservices without DevOps or cloud services  
Microservices are embrace the distributed cloud-native approaches. And you can only maximize benefits of microservices with following these cloud-native principles.
- CI/CD pipeline with devops automations Proper deployment and monitoring tools  
- Managed cloud services to support your infrastructure  
- Key enabling technologies and tools like Containers, Docker, and Kubernetes 
- Following async communications using Messaging and event streaming services
## Monolithic vs MSA Comparison
- Application Architecture
	- Monolith has a simple straightforward structure of one  undivided unit. Microservices have a complex structure that consists of various heterogeneous services and databases.
- Scalability  
	- Monolithic application is scaled as a whole single unit, but microservices can be scaled unevenly. encourages companies to migrate their applications to microservices.
- Deployment
	- Monolithic application provides fast and easy deployment of the whole system. Microservices provides zero-downtime deployment and CI/CD automation.
- Development team
	- If your team doesn't have experience with microservices and container systems, building a microservices-based application will be difficult.