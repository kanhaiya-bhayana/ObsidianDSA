>Redis cache is a caching tool that stores data in a key-value data structure to speed up application.
### What is Redis Cache
- Redis, short for Remote Dictionary Server, is an open-source in-memory data store that can be used as a cache, database, or message broker.
#### How it works
- Redis stores the data in RAM, which is much faster to access the data on a hard drive or SSD.
- It organizes data using key-value pairs, where keys are unique identifiers and values can be strings, lists, sets, and more.
#### Why it's used
> Redis is often used to:
- Speed up website page load time
- Reduce the load on servers
- Provide low-latency access to data
- Improve customer experience 
- Make real-time pricing decisions
- Reduce shopping cart abandonment
##### Add the dotnet package

```sh
$ dotnet add package Microsoft.Extensions.Caching.StackExchangeRedis
```
#### Spin up the container of Redis Cache

docker-compose.yml
```yml
version: "3.8"
services:
  redis:
    image: "redis:latest"
    container_name: redis-cache
    ports:
      - "6379:6379"
    command: ["redis-server", "--requirepass", "password@1234"]
    volumes:
      - redis_data:/data
volumes:
  redis_data:
```

---



---

