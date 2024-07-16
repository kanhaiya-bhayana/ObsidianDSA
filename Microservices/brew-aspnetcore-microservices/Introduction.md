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
