# Introduction
## What is Platform as a Service? (PaaS)
A platform allowing customers to deveop, run, and manage applications without the complexity of building and maintaining the infrastructure typically associated with developing and launching an app. (app-service)



>Choose a platform, upload your code and it runs with little knowledge of the infrastructure. `Not Recommended for "Production" applications` (Enterprise, large companies).

#### Elastic Beanstalk is powered by a CloudFormation template setups for you:
- Elastic Load Balancer
- Autoscaling Groups
- RDS Database
- EC2 Instance preconfigurd (or custom) platforms
- Monitoring (CloudWatch, SNS)
- In-Place and Blue/Green deployment methodologies
- Security (Rotates passwords)
- Can run `Dockerized` environments


## Web vs Worker Environment
#### Web Environment
- for running the web applications
    - Creates an ASG (Auto Scaling Group)
    - Creates an ELB
- Comes in tow variants

#### Worker Environment
- for running the background jobs or the long running jobs.
    - Creates an ASG
    - Creates an SQS Queue
    - Installs teh SQS Daemon on the EC2 instances
    - Creates CloudWatch Alarm to dynamically scale instances based on health.

#### Web Environments 2 Variants
1. Load-Balanced Env
    - Uses ASG and set to scale
    - Uses an ELB
    - Designed to Scale

2. Single-Instance Env
    - Still uses an ASG but Desired Capacity set to 1 to ensure server is always running.
    - No ELB to save on cosst.
    - Public IP Address has to be used to route traffic to server.

## Deployment Policies
There are the Deployment Policies available with Elastic Beanstalk:

Here’s the deployment policy table from the image formatted in Markdown (`.md`) format:

## Elastic Beanstalk Deployment Policies

> **Note:**  
> Rolling requires an ELB because it attaches and detaches instances in batches from the ELB.

| Deployment Policy               | Load-Balanced Env | Single-Instance Env |
|--------------------------------|--------------------|---------------------|
| All at once                    | 👍                 | 👍                  |
| Rolling                        | 👍                 | ❌                  |
| Rolling with additional batch  | 👍                 | ❌                  |
| Immutable                      | 👍                 | 👍                  |
| Traffic splitting              | 👍 (ALB)           | ❌                  |
---

#### 1. All At Once
- Deploys the new app version to all instances at the same time.
- Takes all instances out of service while the deployment processes.
- Servers become available again.
> If you don't configure a deployment policy this will be the default policy used.

**Note**: The fastest but also the most `dangerous` deployment method. In case of failure, you can need to roll back the changes by re-deploying the original version again to all instances.

#### 2. Rolling
- Deploys the new app version to a batch of instances at a time.
- Takes batch's instances out of service while the deployment processes.
- Reattaches updated instances
- Goes onto next batch, taking them out of service
- Reattaches those instances (rinse and repeat)

**Note**: In case of failure, you need to perform an additional roling update in order to roll back the changes.

##### Rolling with Additional Batch
- Launch new Instance that will be used to replace a batch.
- Deploy update app version to new batch.
- Attach the new batch and terminate the existing batch.

> Rolling with Additional Batches ensures our capacity is never reduced. This is important for applications where a reduction in capacity could cause availability for users.

## What is the difference between Rolling and Rolling with Additional Batch?