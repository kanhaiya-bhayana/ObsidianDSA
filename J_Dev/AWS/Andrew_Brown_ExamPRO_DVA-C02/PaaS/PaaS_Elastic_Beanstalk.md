# Platform as a Service (PaaS) – Elastic Beanstalk Overview

## Introduction

**Platform as a Service (PaaS)** is a cloud computing model that provides a ready-to-use platform allowing customers to develop, run, and manage applications without the complexity of building and maintaining the underlying infrastructure. With PaaS, you simply select a platform, upload your code, and the service handles the rest.

> ⚠️ **Note:** PaaS offerings like Elastic Beanstalk are ideal for prototyping or small-to-medium applications. They are generally *not recommended* for large-scale, production-grade enterprise systems due to limited control and configurability.

---
## What is AWS Elastic Beanstalk?

Elastic Beanstalk is a fully managed PaaS offering from AWS. It simplifies the deployment and management of applications by automatically handling the infrastructure provisioning using AWS CloudFormation templates. Key components provisioned include:

* **Elastic Load Balancer (ELB)**
* **Auto Scaling Group (ASG)**
* **Amazon RDS (optional)**
* **EC2 Instances** (preconfigured or custom)
* **Monitoring** (via CloudWatch and SNS)
* **Deployment Strategies** (In-Place, Blue/Green)
* **Security Enhancements** (e.g., password rotation)
* **Docker Support** (can run Dockerized applications)

---
## Environment Types

Elastic Beanstalk offers two environment types based on the nature of your workload:
### 1. Web Environment

* Used for **handling web traffic** and serving web applications.
* Automatically provisions:
  * Auto Scaling Group
  * Elastic Load Balancer (ELB)

**Web Environment Modes:**

* **Load-Balanced Environment**
  * Uses an ELB and ASG with scaling policies
  * Designed for high availability and scalability
* **Single-Instance Environment**
  * Uses ASG (desired capacity = 1)
  * No ELB to reduce cost
  * Requires public IP for direct access
### 2. Worker Environment

* Designed to **process background tasks** and long-running jobs.
* Automatically provisions:
  * Auto Scaling Group
  * Amazon SQS queue
  * EC2 instances with SQS daemon pre-installed
  * CloudWatch Alarms to scale based on job queue length

---
## Deployment Policies

Elastic Beanstalk supports various deployment strategies, especially useful in Load-Balanced environments. Below is a comparison of supported strategies:
### Supported Deployment Policies

> ⚠️ **Note:** Rolling deployments require an ELB to attach/detach instances in batches.

| Deployment Policy             | Load-Balanced Environment | Single-Instance Environment |
| ----------------------------- | ------------------------- | --------------------------- |
| All at Once                   | ✅                         | ✅                           |
| Rolling                       | ✅                         | ❌                           |
| Rolling with Additional Batch | ✅                         | ❌                           |
| Immutable                     | ✅                         | ✅                           |
| Traffic Splitting             | ✅ (with ALB)              | ❌                           |

---
### 1. **All at Once**

* Deploys the new version to *all instances simultaneously*.
* Instances are **taken out of service** during the deployment.
* Fastest strategy, but **riskiest** in terms of service interruption.
* Default policy if none is specified.

> ⚠️ If a deployment fails, you must manually re-deploy the previous version across all instances.

---
### 2. **Rolling**

* Deploys the application in **batches**.
* A batch of instances is taken out of service, updated, and returned before the next batch is updated.
* Reduces downtime, but can lead to capacity drops during deployment.

> ⚠️ Rollbacks require performing another rolling update to revert.

---
### 3. **Rolling with Additional Batch**

* Launches a new batch of instances before terminating old ones.
* Ensures application **capacity is not reduced** during deployment.
* Ideal for applications where **availability is critical**.

---
## Difference Between Rolling and Rolling with Additional Batch

| Feature                          | Rolling | Rolling with Additional Batch |
| -------------------------------- | ------- | ----------------------------- |
| Reduces capacity during deploy   | ✅ Yes   | ❌ No                          |
| Launches additional instances    | ❌ No    | ✅ Yes                         |
| Safer for high-availability apps | ❌ No    | ✅ Yes                         |
| Complexity                       | Simple  | Slightly more complex         |
## EB - Immutable
- Creates a new ASG group with EC2 instances.
- Deploy the updated version of the app on the new EC2 instances.
- Point the ELB to the new ASG and delete the old ASG which will terminate the old EC2 instances.

```
The safest way to deploy for critical applications.

**Note**: In case of failure, you just terminate the new instances since the existing instances still remain.
```
## EB – Deployment Methods

| **Method**                   | **Impact of Failed Deployment**                                            | **Deploy Time**       | **Downtime**   | **DNS Change**     | **Rollback Process**                | **Code Deployed to Instances** |
|-------------------------------|----------------------------------------------------------------------------|------------------------|----------------|--------------------|------------------------------------|---------------------------------|
| **All-at-once**              | Downtime                                                                   | ⏱                     | 🔴 Yes         | 🔷 No               | Manual                             | Existing                        |
| **Rolling**                  | Single batch out of service; successful batches before failure remain active | ⏱⏱⏱ †            | 🟢 No          | 🔷 No               | Manual                             | New + Existing                  |
| **Rolling + Additional Batch** | Minimal if first batch fails; otherwise, similar to *Rolling*             | ⏱⏱⏱ †            | 🟢 No          | 🔷 No               | Manual                             | New + Existing                  |
| **Immutable**                | Minimal                                                                    | ⏱⏱                   | 🟢 No          | 🔴 Yes             | Terminate new                      | New                             |
| **Blue/Green**               | Minimal                                                                    | ⏱⏱                   | 🟢 No          | 🔴 Yes             | Swap URL                           | New                             |
| **Traffic Splitting**        | Temporary impact as traffic shifts between versions                        | ⏱⏱⏱ ††          | 🟢 No          | 🔷 No               | Reroute & terminate new           | New                             |

---
### Legend:
- **Deploy Time:**
  - ⏱ = Short
  - ⏱⏱ = Moderate
  - ⏱⏱⏱ = Long
- 🔴 = Yes
- 🟢 = No
- 🔷 = No Change

† Time may vary  
†† Time varies depending on evaluation time option setting
##### Improvements include:

* Consistent column widths.
* Clear ✅/🔴/🟢 symbols for Yes/No for quick scanning.
* Added a *Legend* section at the bottom for symbols.
* Replaced some wordiness with clearer phrases.
## EB - In-Place vs Blue/Green Deployment
> Elastic Beanstalk by default performs in-place updates.

> In-Place and Blue/Green Deployment are not definitive in definition and the context can change the scope of what they mean.

```
In-Place could mean within **the scope of Elastic Beanstalk Env**

All the `deployment policies provided by EB could be considered In-Place since they are within the scope of a single EB environment.

- All at once
- Rolling
- Rolling with additional batch
- Immutable
```

```
In-Place could mean within **the scope of the same server** (not replacing the server)

Deployment policies which do not involve the server being replaced

- All at once
- Rolling
```

```
In-Place could mean wihin **the scope of an uninterrupted server**

Traffic is never routed away from the server (taken-of-service). Implements **Zero-downtime** deploys where Blue/Greens occurs on the server.

EB can't do this. Capistrano + Ruby on Rails + Unicorn is famous case fo this method of deployment.
```