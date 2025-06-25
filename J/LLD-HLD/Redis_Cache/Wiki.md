# 📦 How to Configure Redis Cache in Spring Boot 3 Microservice

> ✅ This guide explains how to integrate Redis-backed caching into a Spring Boot 3 microservice using Spring Cache abstraction.

---
## 🛠️ 1. Add Required Dependencies

Add the following to your `pom.xml`:

```xml
<!-- Caching abstraction -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>

<!-- Redis support (uses Lettuce driver by default) -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

---
## ⚙️ 2. Configure Redis in `application.yml`

```yaml
spring:
  cache:
    type: redis
    redis:
      time-to-live: 10m       # default TTL for all entries
      cache-null-values: false

  data:
    redis:
      host: localhost
      port: 6379
      # password: secret       # if applicable
```

---
## 🧠 3. Enable Caching and Define Cache Config

```java
@Configuration
@EnableCaching
public class CacheConfig {

    @Bean
    public RedisCacheConfiguration redisCacheConfiguration() {
        return RedisCacheConfiguration.defaultCacheConfig()
            .disableCachingNullValues()
            .serializeValuesWith(
                RedisSerializationContext.SerializationPair
                    .fromSerializer(new GenericJackson2JsonRedisSerializer())
            )
            .entryTtl(Duration.ofMinutes(10));
    }
}
```

---
## 🧩 4. Use Cache Annotations in Services

```java
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository repo;

    @Cacheable(value = "users", key = "#id")
    public UserDto getUser(Long id) {
        return repo.findById(id).orElseThrow();
    }

    @CachePut(value = "users", key = "#result.id")
    public UserDto updateUser(UserDto dto) {
        return repo.save(dto.toEntity()).toDto();
    }

    @CacheEvict(value = "users", key = "#id")
    public void deleteUser(Long id) {
        repo.deleteById(id);
    }
}
```
### Annotation Summary

|Annotation|Purpose|When It Triggers|
|---|---|---|
|`@Cacheable`|Reads from cache|Before method execution|
|`@CachePut`|Updates cache|After method execution|
|`@CacheEvict`|Removes entry from cache|After method execution|

---
## 🧪 5. Optional: Test the Caching Behavior

```java
@SpringBootTest
class UserServiceTest {

    @Autowired UserService userService;
    @SpyBean UserRepository repo;

    @Test
    void verifyCacheIsUsed() {
        userService.getUser(1L); // Hits DB
        userService.getUser(1L); // Returns from cache

        verify(repo, times(1)).findById(1L);
    }
}
```

---
## 🔍 6. Redis CLI for Debugging

> Redis CLI helps inspect keys and values cached by Spring Boot.
### Common Commands

```bash
# Connect to local Redis
redis-cli

# List keys by pattern
SCAN 0 MATCH users::* COUNT 100

# Read a cached value
GET users::1

# Check TTL
TTL users::1

# Delete specific cache entry
DEL users::1
```

> ⚠ Note: Spring constructs keys as `cacheName::key`, so `users::1` is for `@Cacheable(value = "users", key = "#id")`.

---
## 🧯 7. Optional: Run Redis Locally with Docker

```bash
docker run -d --name redis -p 6379:6379 redis:7
```

---
## 📌 Checklist for Production

|Concern|Recommendation|
|---|---|
|Security|Use TLS and Redis AUTH|
|High Availability|Use Redis Cluster or managed Redis (AWS/GCP)|
|Monitoring|Enable Micrometer Redis metrics|
|Serialization|Use JSON (as shown above)|
|Eviction Policy|Set Redis `maxmemory-policy` to `allkeys-lru`|

---
## ✅ Summary

- Add dependencies for Redis and caching
- Enable caching with `@EnableCaching`
- Use `@Cacheable`, `@CachePut`, `@CacheEvict` in services
- Monitor and debug using `redis-cli`