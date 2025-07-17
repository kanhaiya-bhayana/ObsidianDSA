### Mock Behavioral Interview: Publicis Sapient

#### **Q1. Tell me about a time when you had to work with a difficult team member. How did you handle the situation, and what was the outcome?**

During my time at Incedo, I was part of a team responsible for developing a microservices-based Business Assessment Tool (BAT) for AssetMark’s e-Wealth Manager (EWM) platform. One of my teammates was responsible for Kafka integration but frequently missed stand-ups and delayed deliverables, which created a bottleneck.

Instead of escalating, I had a 1-on-1 conversation and discovered they were overwhelmed but hesitant to ask for help. I offered to pair-program and shared Kafka best practices. We resolved the integration issues and completed the feature on time. The teammate became more engaged and later contributed significantly to caching optimization.

This taught me the value of empathy, proactive communication, and fostering a collaborative team environment.

---

#### **Q2. Describe a time when you had to learn a new technology or tool quickly to complete a project. How did you approach the learning curve, and what was the result?**

While working on the BAT module, I had to integrate our service with Azure Event Hub using Apache Kafka. Though I had limited experience with Kafka, the integration was critical.

I quickly went through Kafka’s documentation, built a small proof-of-concept, and studied previous internal implementations. Within a few days, I completed the integration and ensured seamless real-time communication of user preferences.

I documented the process and helped teammates avoid similar issues. The project remained on schedule, and the experience boosted my confidence in learning under pressure.

---

#### **Q3. Tell me about a time when a project didn’t go as planned. How did you respond, and what did you learn?**

During the BAT development, we faced issues integrating our new microservice with a legacy email service. Notifications were delayed or missing due to mismatched payload formats.

I initiated a root cause analysis, collaborated with the legacy team, and fixed the issue by updating payloads, adding logging and retry mechanisms, and aligning expectations. The issue was resolved in the next sprint.

I learned the importance of cross-team communication, early contract validation, and taking end-to-end ownership.

**Follow-up Response:**  
Yes, ideally the issue could have been caught during unit testing. However, we lacked access to the legacy staging environment and relied on outdated documentation. After resolving the issue, I introduced contract validation steps and coordinated test environment access. This improved our integration process going forward.

---

#### **Q4. Describe a time when you took ownership of a project or task beyond your usual responsibilities. What motivated you, and what was the result?**

While working on the Client Section module of EWM, I noticed ambiguity in the product requirements. Though my role was backend-focused, I proactively reached out to the product owner, clarified use cases, and created detailed functional breakdowns.

This helped align our frontend and QA teams, reduced rework, and improved sprint velocity.

Additionally, during my trainee period, I volunteered to lead the demo presentation to our Chief Delivery Officer. It was a high-stakes moment, and I ensured the team’s contributions were clearly represented, which boosted morale.

These experiences taught me that ownership is about enabling your team and delivering value, not just sticking to your assigned tasks.

---

#### **Q5. Tell me about a time when you had to communicate complex technical information to a non-technical stakeholder. How did you ensure they understood, and what was the outcome?**

During the chatbot integration for the EWM platform, we needed to secure communication using JWT with RSA256 and KID support. While technically crucial, product stakeholders were unclear why this complexity was necessary.

I translated the technical reasons into business impact: improved security, stateless scaling, and audit readiness. I also presented a visual diagram of the JWT verification flow.

As a result, stakeholders fully supported the design, and the integration rolled out smoothly. This reinforced that clear, audience-tailored communication is just as important as the technical solution itself.