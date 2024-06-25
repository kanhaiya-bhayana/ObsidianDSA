
## Observability

- Observability is the ability to understand the internal state of a system by observing its outputs. 
- In the context of microservices, observability is achieved by collecting and analyzing data from a variety of sources, such as metrics, logs and traces.


#### The three pillars of observability are:
1. **Metrics:** Metrics are quantitative measurement of the health of a system. They can be used to track things like CPU usage, memory usage, and response times.
2. **Logs:** Logs are a record of events that occur in a system. They can be used to track things like errors, exceptions, and other unexpected events.
3. **Traces:** Traces are a record of the path that a request takes through a system. They can be used to track the performance of a request and to identify bottlenecks.

> By collecting and analyzing data from these three sources, you can gain a comprehensive understanding of the internal state of your microservices architecture. This understanding can be used to identify and troubleshoot problems, improve performance, and ensure the overall health of your system.


## Monitoring

- Monitoring in microservices involves checking the telemetry data available for the application and defining alerts for known failure states. 
- This process collects and analyzes data from a system to identify and troubleshoot problems, as well as track the health of individual microservices and the overall health of the microservices network.

#### Monitoring in microservices is important because it allows you to:

1. **Identify and troubleshoot problems:** By collecting and analyzing data from your microservices, you can identify problems before they cause outages or other disruptions.
2. **Track the health of your microservices:** Monitoring can help you to track the health of your microservices, so you can identify any microservices that are underperforming or that are experiencing problems.
3. **Optimize your microservices:** By monitoring your microservices, you can identify areas where you can optimize your microservices to improve performance and reliability.

> Monitoring and observability can be considered as two sides of the same coin. Both rely of the same types of telemetry data to enable insight into software distributed systems. Those data types - metrics, traces, and logs - are often referred to as the three pillars of observability. 


## Monitoring vs Observability


| **Feature** | **Monitoring**                     | **Observability**                             |
| ----------- | ---------------------------------- | --------------------------------------------- |
| Purpose     | Identify and troubleshoot problems | Understand the internal state of a system     |
| Data        | Metrics, traces, and logs          | Metrics, traces, logs, and other data sources |
| Goal        | Identify problems                  | Understand how a system works                 |
| Approach    | Reactive                           | Proactive                                     |

#### So what is reactive approach?

When I say reactive, we are going to react when a problem occurs. For example, your operations team can react when a particular microservice is facing some network problems or some performance issues. Without seeing those events. Your operations team cannot react to them. That's why inside the monitoring the approach will always be reactive

#### So what is proactive approach?

For example, your microservice might be throwing some NullPointerExceptions. Maybe it is not throwing for each and every request for some random scenario or for some rare scenario. A particular microservice is throwing a RuntimeException or NullPointerException. So as a developer you will try to proactively identify the issues so that you can provide a fix in the future release. So this is a proactive approach.

> In other words, monitoring is about collecting data and observability is about understanding data.
> 
> Monitoring is reactive to problems while observability is fixing them in real time.

