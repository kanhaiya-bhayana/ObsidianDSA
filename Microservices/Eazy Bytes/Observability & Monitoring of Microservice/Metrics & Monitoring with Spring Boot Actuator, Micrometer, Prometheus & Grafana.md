Events logs are essential for monitoring applications, but they don't provide enough data to answer all of the questions we need to know. To answer questions like CPU usage, memory usage, threads usage, error requests etc. & properly monitor, manage, and troubleshoot an application in production, we need more data.

Metrices are numerical measurements of an application's performance, collected and aggregated at regular intervals. They can be used to monitor the application's health and performance, and to set alerts or notifications when thresholds are exceeded.


#### 1. Actuator
Actuator is mainly used to expose operational information about the running applications health, metrics, info, dump, env, etc. It uses HTTP endpoints or JMX beans to enable us to interact with it.

#### 2. Micrometer
Micrometer automatically exposes/actuator/metrics data into something your monitoring system can understand. All you need to do is include that vendor-specific micrometer dependency in your application. Think SLF4J, but for metrics.


> SLF4J - Simple Logging Framework For Java


#### 3. Prometheus
The most common format for exporting metrics is the one used by Prometheus, which is "an open-source systems monitoring and altering toolkit". Just as Loki aggregates and stores event logs, Prometheus does the same with metrics.

Prometheus is an open source product which is going to help you to collect all the metrics from the respective containers and microservices. We can also try to build the dashboards, graphs, which will help us to monitor our microservices. So it is a open source monitoring solution.

#### 4. Grafana
Grafana is a visualization tool that can be used to create dashboards and charts from Prometheus data.