# AWS Service Overview

A concise summary of key AWS services, their primary functions, and relevant comparisons to Azure equivalents.

---

## Networking & Delivery

- **DNS Service:**  
  **Route 53**  
  - Highly available and scalable DNS web service for domain registration, DNS routing, and health checks[1][12].
  - Supports advanced routing (latency, geoproximity, weighted), DNS failover, private DNS for VPC, and integration with ELB and CloudFront[12].

- **Content Delivery Network:**  
  **CloudFront**  
  - Global CDN that caches content at edge locations for low-latency, high-speed delivery.

---

## Load Balancing

- **Elastic Load Balancer (ELB):**  
  Distributes incoming traffic across multiple targets for high availability and fault tolerance.
  - **Application Load Balancer (ALB):**  
    Operates at Layer 7 (HTTP/HTTPS), supports path/host-based routing, WebSockets, HTTP/2, and Lambda targets[2].
  - **Network Load Balancer (NLB):**  
    Operates at Layer 4 (TCP/UDP), designed for high performance and low latency[2].
  - **Classic Load Balancer:**  
    Legacy option, only for EC2-Classic networks[2].

---

## Compute

| Service            | Description                                                                                  | Azure Equivalent    |
|--------------------|----------------------------------------------------------------------------------------------|---------------------|
| **EC2**            | Virtual servers (VMs) with scalable compute capacity[3].                                     | Virtual Machines    |
| **AWS Lambda**     | Serverless compute, runs code in response to events, no server management required[4].       | Azure Functions     |

---

## Containers & Orchestration

| Service | Description                                                                                  |
|---------|----------------------------------------------------------------------------------------------|
| **ECS** | Managed container orchestration for Docker containers; simple, AWS-native[5].                |
| **EKS** | Managed Kubernetes service for advanced, highly configurable container orchestration[5].      |

- **Comparison:**  
  - ECS: Easier to start, lower operational overhead, tightly integrated with AWS[5].
  - EKS: Kubernetes-compatible, more flexible but with a steeper learning curve[5].

---

## Identity & Access

- **Amazon Cognito:**  
  Provides user authentication, authorization, and user management for web/mobile apps. Supports social logins, passwordless authentication, and integrates with AWS Lambda for custom flows[6].  
  - **Azure Equivalent:** Microsoft Entra ID

---

## Caching

| Service         | Description                  |
|-----------------|-----------------------------|
| **ElastiCache** | Managed in-memory caching.  |
| - Memcached     | Simple, distributed caching.|
| - Redis         | Advanced, persistent cache. |

---

## Databases

| Service             | Description                                                                                       |
|---------------------|---------------------------------------------------------------------------------------------------|
| **RDBMS**           | **Amazon Aurora:** AWS-built, MySQL/PostgreSQL-compatible, high performance.                      |
| **RDS**             | Managed relational DB service supporting MySQL, PostgreSQL, SQL Server, MariaDB, Oracle.          |
| **NoSQL**           | **DynamoDB:** Managed NoSQL database. <br> **DocumentDB:** MongoDB-compatible managed database.   |

---

## Search & Analytics

- **OpenSearch:**  
  Open-source search and analytics suite, forked from Elasticsearch. Scalable, supports structured/unstructured data, used for search, observability, and analytics.

---

## Packaged Infrastructure / Platform Services

- **Packaged Infrastructure:**  
  Pre-configured, ready-to-use infrastructure components, often managed as code (e.g., AWS CloudFormation)[Azure App Service]. Enables repeatable, scalable deployments.

| Service             | Description                                                                                                           | Azure Equivalent    |
|---------------------|-----------------------------------------------------------------------------------------------------------------------|---------------------|
| **Elastic Beanstalk** | PaaS for deploying and managing applications without managing infrastructure.                                         | App Service         |
| **App Runner**        | Fully managed service for building, deploying, and scaling containerized web apps and APIs directly from code/images. |
| **Lightsail**         | Simplified cloud platform for VPS, containers, databases, storage—ideal for small businesses and cloud beginners.     |

---

## API & Data Services

- **GraphQL as a Service:**  
  **AppSync** – Managed GraphQL service for real-time data queries, synchronization, and offline access.

---

## DevOps & CI/CD

| Service           | Description                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------|
| **CodeCommit**    | Managed Git repositories for source control; supports collaboration, encryption, and IAM-based access[7][13].   |
| **CodeBuild**     | Fully managed CI service for compiling, testing, and packaging code; supports custom and pre-configured images[8].|
| **CodeDeploy**    | Automated deployments to EC2, Lambda, and ECS; supports blue/green and rolling deployments[9].                  |
| **CodePipeline**  | Orchestrates CI/CD workflows, automating build, test, and deployment stages with integration to AWS services[10].|

---

## Monitoring & Auditing

| Service           | Description                                                                                                     | Azure Equivalent    |
|-------------------|-----------------------------------------------------------------------------------------------------------------|---------------------|
| **CloudWatch**    | Monitoring and observability for AWS and hybrid resources; collects metrics, logs, events, and enables alarms[11].| Azure Monitor       |
| **CloudWatch Insights** | Advanced analytics for logs and metrics, enabling deep operational insights.                               |                     |
| **CloudTrail**    | Records and audits API calls and account activity for governance, compliance, and operational auditing.         |                     |

---

## Notes

- **Service Names:** AWS service names are often prefixed with "Amazon" or "AWS" (e.g., Amazon EC2, AWS Lambda).
- **Integration:** Most AWS services are designed to integrate seamlessly with each other for building scalable, secure, and highly available applications.
- **Azure Equivalents:** Where relevant, Azure equivalents are noted for cross-cloud comparison.

