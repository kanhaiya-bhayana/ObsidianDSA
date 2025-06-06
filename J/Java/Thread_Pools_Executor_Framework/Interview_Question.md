## Why you have taken corePoolSize as 2, why not 10 or 15 or another number, what's the logic?

### Answer 
Generally, the ThreadPool min and max size are depend on various factors like:
- CPU Cores
- JVM Memory
- Task Nature (CPU Intensive or I/O Intensive)
- Concurrency Requirement (Want high or medium or low concurrency)
- Memory Required to process a request
- Throughput etc.

And it is an iterative process to update the min and max values based on monitoring.

#### Formula to find the max no. of thread
> Max no. of thread = No. of CPU Core * (1 + Request waiting time / processing time)

#### Max no. of active tasks
> Max no. of active tasks = Task arrival rate * Task execution time