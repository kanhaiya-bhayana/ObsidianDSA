## Q1. Tell me about a time when you had to work with a difficult team member. How did you handle the situation, and what was the outcome?

>During my time at Incedo, I was part of a team responsible for developing a microservices-based **Business Assessment Tool (BAT)**. One of my teammates was responsible for integrating Kafka and publishing user preferences to Azure Event Hub. However, they often delayed tasks and were unresponsive during our sprint stand-ups, which created a bottleneck for the rest of us—especially when syncing microservices and coordinating deployments.
>
>Instead of escalating immediately, I reached out privately to understand if they were facing any blockers. It turned out they were struggling with some Kafka configuration issues and felt uncomfortable asking for help, thinking it would reflect poorly on their performance.
>
>I offered to pair-program and walked them through some Kafka integration best practices I had learned earlier while building a similar pipeline. Together, we resolved the issues, and I also created internal documentation for the team to reference in the future.
>
>As a result, not only did we meet our sprint goals on time, but the team dynamic also improved significantly.

---
## ✅ Q2. Describe a time when you had to learn a new technology or tool quickly to complete a project. How did you approach the learning curve, and what was the result?

> While working on the **Business Assessment Tool (BAT)** project at Incedo, we had to integrate our microservices with **Azure Event Hub** using **Apache Kafka** for real-time data streaming. Although I had some exposure to messaging systems, I had never worked with Kafka in a production-grade environment before, and the integration was on the critical path of the project.
> 
> Recognizing the urgency, I broke the task into manageable parts and started with official documentation and tutorials. I also reviewed logs and previous internal implementations to understand how Kafka was being used elsewhere in our org. I built a small proof-of-concept Kafka producer and consumer to simulate publishing user preferences, which helped solidify my understanding.
> 
> Within a few days, I was able to confidently integrate Kafka with our service and ensure seamless publishing of user preferences to **Azure Event Hub**. I also documented the configuration and common pitfalls, which later helped a teammate avoid the same learning curve.
> 
> The feature was delivered on time, and the successful integration enabled us to automate user-based workflows — improving system responsiveness and reducing manual effort. This experience reinforced my confidence in learning fast and contributing under tight timelines.
---
## ✅ Q3. Tell me about a time when a project didn’t go as planned. How did you respond, and what did you learn from the experience?

> While working on the **Business Assessment Tool (BAT)** module for **AssetMark’s EWM (e-Wealth Manager)** platform, we faced an unexpected challenge during integration with a **legacy email notification service**. This service was responsible for triggering real-time updates based on user inputs, and it played a critical role in streamlining advisor workflows.
> 
> Initially, everything looked good during development. But after deployment to a staging environment, we discovered that notifications were either delayed or not being sent at all. Upon investigation, we realized that there were inconsistencies in the payload format expected by the legacy system and what our new microservice was generating—especially around headers and encoding.
> 
> Recognizing the urgency, I led a focused debugging session and coordinated with the legacy system owner to understand the contract mismatches. I created test payloads, enhanced logging, and implemented a retry mechanism to ensure reliability. Additionally, we aligned the event schema across systems and updated the documentation for future developers.
> 
> The fix was deployed in the next sprint, and we saw a clear improvement in system behavior. This issue taught me two key lessons: the importance of **communication**, and the need for **collaborative problem-solving** when working across both modern and legacy stacks.
> 
> Most importantly, it reinforced that even in a fast-paced Agile environment, **taking ownership** and **staying calm under pressure** makes a huge difference in delivering value.

---
### 🔄 Likely Follow-Up Question:

> _“If there were inconsistencies in the payload format, shouldn’t that have been caught during development or unit testing?”_

---
### ✅ Suggested Response:

> In this case, one of the key reasons it wasn’t detected during unit testing was because the **legacy notification service was managed by a separate team** and we didn’t have direct access to their staging environment or fully documented payload schemas at the time. Our mocks and assumptions were based on old documentation, which later turned out to be **outdated and partially incorrect**.
> 
> After this incident, I proposed a more robust **contract validation step** as part of our integration testing process. We also coordinated with the legacy system team to **align on payload contracts** and establish **shared test environments** for early compatibility checks.
> 
> This experience actually pushed us to adopt **contract-first development** in later modules, which greatly reduced integration issues. It was a learning moment, and I took accountability by not just fixing the problem, but improving the overall process.

---
## ✅ Q4. Describe a time when you took ownership of a project or task beyond your usual responsibilities. What motivated you, and what was the result?

### Redis Cache

**STAR format**

---

### ⭐ **Situation**

In one of my projects, the onshore team had developed a shared Redis cache library for all modules. The library exposed methods like `GetAsync`, `GetOrPutAsync`, `PutAsync`, and `IsAlive`.

While integrating this library into our module, we encountered a common use case: we needed to invalidate or remove a specific cache entry by its key. However, the library did not provide a generic method for this, so each consumer team was forced to implement their own cache invalidation logic repeatedly.


---

### 🔷 **Task**

We needed to invalidate or remove a specific cache entry by its key. However, the library did not provide a generic method for this, so each consumer team was forced to implement their own cache invalidation logic repeatedly.


---

### 🛠️ **Action**

I identified this gap and proposed to the onshore team that we enhance the shared library by adding a reusable method called `PurgeCacheAsync`, which would accept a key and remove the corresponding cache entry. After getting their approval, I implemented the method directly in the library, ensuring it was thoroughly tested and followed the same design patterns as the existing methods.

---

### 📈 **Result**

By adding `PurgeCacheAsync` to the shared library, we eliminated duplicate code across modules, improved maintainability, and ensured consistent behavior for cache invalidation. This change saved developer time, reduced the risk of errors, and made the library more complete and user-friendly for all teams.

---

### 🎤 **Spoken Version**

> In one of my projects, the onshore team had developed a shared Redis cache library that provided methods like `GetAsync`, `GetOrPutAsync`, `PutAsync`, and `IsAlive`. While integrating this library into our module, I noticed a gap: there was no generic way to invalidate or remove a cache entry by key. As a result, every team consuming the library was writing their own custom invalidation logic, which led to duplication and inconsistencies.
>
> I proposed to the onshore team that we enhance the library by adding a reusable method called `PurgeCacheAsync` to handle key-based invalidation. After getting their approval, I implemented the method in the library, aligned with its existing design patterns, and ensured it was thoroughly tested.
>
> This improvement made the library more robust and user-friendly, eliminated redundant code across modules, and saved developer time while ensuring consistent cache invalidation logic.


---
## ✅ Q5. Tell me about a time when you had to communicate complex technical information to a non-technical stakeholder. How did you ensure they understood, and what was the outcome?

> While working on the **e-Wealth Manager (EWM)** platform at AssetMark, we were tasked with integrating a **chatbot** to enhance advisor support and client interaction. To secure the communication between the chatbot and our microservices, I implemented **JWT-based authentication** using the **RSA256 signature algorithm** along with **Key ID (KID)** support.

> While this was a technically critical feature, it wasn't easy for our product stakeholders to understand why we couldn’t just use simpler session tokens or API keys. Instead of diving deep into encryption or security layers, I took the approach of **explaining the "why" in business terms**.
> 
> I described how JWT with RSA256 would:
> 
> - **Ensure end-to-end trust** between the chatbot and our backend services
>     
> - **Avoid session management overhead**, making the system more scalable
>     
> - **Comply with enterprise-grade security requirements**, which were a priority for future audits
>     
> 
> I also created a **visual diagram** showing how a JWT is signed and verified, and how the KID enables key rotation without downtime. This helped non-technical stakeholders clearly see the value and risks avoided.
> 
> The explanation helped gain full stakeholder support, and the integration was rolled out smoothly. This experience taught me that **clear communication, tailored to the audience**, is crucial—especially when bridging tech decisions with business impact.

---
