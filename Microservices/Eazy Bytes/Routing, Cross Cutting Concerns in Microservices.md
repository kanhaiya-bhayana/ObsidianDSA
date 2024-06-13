
## 1. HOW DO WE MAINTIAN A SINGLE ENTRYT POINT INOT MICROSERCIES NETWORK?
	
	How do we build a single gatekeeper for all the inbound traffic to our microservices.

	This way the client doesn't need to keep track of the different services involved in a transaction, simplifying the client's logic.


## 2. HOW DO WE HANDLE CROSS CUTTING CONCERNS?

	1. In a distributed microservices architecture, 
		how do we make sure to have a consistently enforced cross cutting concerns like logging, auditing, tracing and security across multiple microservices.

## 3. HOW DO WE ROUTE BASED ON CUSTOM REQUIREMENTS?

	1. How to provide dynamic routing capabilities which allows to define routing rules based on various criteria such as HTTP headers, request parameters etc.
	2. Inside microservices work?



## SOLUTION or STANDARD

		These challanges in microservices can be solved using a 
		"EDGE SERVER" or can say "API GATEWAY" or "GATEWAY"

		So whatever the name it is, since this server is going to sit on the edge of your microservice network and monitor all the incoming and outgoing requests.
		That's why we call this as Edge server. 


It is important to Generalize, instead of writing the same business logic in each microservice for security, logging and tracing.

- Edge servers are applications positioned at edge of a system, responsible for implementing functionalities such as API gateways and handling cross-cutting concerns. 
- By utilizing edge servers, it becomes possible to prevent cascading failures when invoking downstream services, allowing for the specification of retries and timeouts for all internal service calls.
- Additionally, these servers enable control over ingress traffic, empowering the enforcement of quota policies.
- Furthermore, authentication and authorization mechanisms can be implemented at the edge, enabling the passing of tokens to downstream services for secure communication and access control.


## Few important tasks that API Gateway does

![[Pasted image 20240608141216.png]]

