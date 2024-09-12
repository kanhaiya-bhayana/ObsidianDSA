#### Section Overview
- Serverless Overview
- Serverless Services in Azure
- Azure Functions Refresher
- Introducing Durable Functions
- Durable Functions Common Workflows
- Durable Function Types


#### What is Serverless

What it means when someone says they are going serverless.?

As an example, consider a scenario where you intend to design a straightforward to do list application. The frontend of the application communicates with the backend through rest APIs. These APIs invoke the relevant business logic and code to store and retrieve data in a database. Users can access their to do lists via mobile application, Android, iOS or a web application.

To implement this in a traditional server based model, you would have web servers where you write code to accept incoming rest requests and certain endpoint URLs. You would then write code on the same or another server that runs your business logic in the web server layer.

Additionally, you would have another server running a database service such as MySQL or MongoDB to store your data in a server based model. All of this infrastructure could be virtual machines running in cloud or in your on premise infrastructure.

As your application grows and demand increases, you need to add more servers to scale. However, when demand decreases, you need to remove the extra servers to avoid wasting resources and incurring unnecessary costs.

This means you have to be careful not to overprovision or under-provision your server capacity. Additionally, you need to keep the servers up to date with the latest OS security updates and software updates while making sure not to break anything. These are just a few of the numerous obstacles that come with hosting via a server centric model.

On the other hand, in a serverless model, you don't have to manage servers for your application. In state, you can use various serverless services like Azure Functions, AWS, Lambda to run code on demand in a serverless database service like Cosmos DB to store data. The cloud provider such as Essure, takes care of the underlying server infrastructure for you and can automatically scale to accommodate millions of requests and demand. 

Additionally, the most crucial aspect is that you are only charged for the duration your court runs. Unlike a virtual machine where you pay for the entire time, the virtual machine is operational regardless of whether it is in use or not. This is a very high level comparison between server and serverless model.

An increasing number of businesses are adopting a serverless approach as it offers improved efficiency and cost advantages.
