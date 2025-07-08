# 📘 Spring Boot: Managing Environment Variables & AWS Secrets Manager Integration

This document explains best practices for managing configuration and secrets in a Spring Boot application, and how to securely integrate with **AWS Secrets Manager**.

---

## 🔷 Where to Keep Environment Variables and Configurations?

Spring Boot supports multiple ways to configure your application.

### 1️⃣ `application.properties` / `application.yml`

✅ Recommended for development and default configurations.
Place this file in `src/main/resources/`.

**Example: `application.properties`**

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=myuser
spring.datasource.password=secret
server.port=8080
```

**Example: `application.yml`**

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: myuser
    password: secret
  server:
    port: 8080
```

---

### 2️⃣ Profiles for Multiple Environments

Use profiles for dev/test/prod setups.

* `application-dev.properties`
* `application-prod.properties`
* `application-test.properties`

Activate a profile:

```properties
spring.profiles.active=dev
```

---

### 3️⃣ Environment Variables

You can also externalize sensitive data as environment variables.

✅ Example: `SPRING_DATASOURCE_USERNAME`, `SPRING_DATASOURCE_PASSWORD`.

---

### 4️⃣ Command-line Arguments

Pass values during startup:

```bash
java -jar app.jar --spring.datasource.url=jdbc:mysql://...
```

---

### 5️⃣ Vault/Secret Manager

For production, sensitive data like database credentials should **not** be stored in `application.properties` or Git. Instead, use a secure secret manager such as:

* AWS Secrets Manager
* HashiCorp Vault
* Azure Key Vault

---

# 🌟 Why AWS Secrets Manager?

✅ Secure storage of sensitive data (DB passwords, API keys, etc.)
✅ Automatic credential rotation (optional).
✅ Fine-grained IAM permissions.
✅ Remove secrets from source code and Git.

---

# 🚀 How to Integrate AWS Secrets Manager with Spring Boot?

You can integrate AWS Secrets Manager in **two recommended ways**:

---

## 📝 Option 1: Use AWS SDK Directly

### 📦 Dependency

```xml
<dependency>
    <groupId>software.amazon.awssdk</groupId>
    <artifactId>secretsmanager</artifactId>
</dependency>
```

### 🧑‍💻 Example Code

```java
import software.amazon.awssdk.auth.credentials.DefaultCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.secretsmanager.SecretsManagerClient;
import software.amazon.awssdk.services.secretsmanager.model.GetSecretValueRequest;
import software.amazon.awssdk.services.secretsmanager.model.GetSecretValueResponse;

public class AwsSecretsFetcher {

    public static String getSecret(String secretName, Region region) {
        SecretsManagerClient client = SecretsManagerClient.builder()
            .region(region)
            .credentialsProvider(DefaultCredentialsProvider.create())
            .build();

        GetSecretValueRequest getSecretValueRequest = GetSecretValueRequest.builder()
            .secretId(secretName)
            .build();

        GetSecretValueResponse getSecretValueResponse = client.getSecretValue(getSecretValueRequest);

        return getSecretValueResponse.secretString();
    }
}
```

Call it:

```java
String dbCredentialsJson = AwsSecretsFetcher.getSecret("mydb-secret", Region.US_EAST_1);
```

Parse the returned JSON and use the values.

---

## 📝 Option 2: Spring Cloud AWS

If you already use Spring Cloud, this is the simplest way.

### 📦 Dependencies

```xml
<dependency>
    <groupId>io.awspring.cloud</groupId>
    <artifactId>spring-cloud-aws-starter-secrets-manager</artifactId>
</dependency>
```

With BOM:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.awspring.cloud</groupId>
            <artifactId>spring-cloud-aws-dependencies</artifactId>
            <version>3.1.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

---

### 🔷 `application.properties`

```properties
spring.cloud.aws.region.static=us-east-1
spring.config.import=aws-secretsmanager:/your-secret-name
```

If your secret in AWS looks like this:

```json
{
  "spring.datasource.username": "myuser",
  "spring.datasource.password": "mypassword"
}
```

Spring Boot will automatically bind these to your application’s configuration.

---

# 👌 Best Practices

✅ Use IAM roles (on EC2, ECS, Lambda) to avoid hardcoding AWS credentials.
✅ Structure secrets in JSON to map easily to Spring properties.
✅ Enable automatic secret rotation if needed.
✅ Never commit secrets to source control.

---

# 💡 Summary Table

| Feature                    | AWS SDK Direct | Spring Cloud AWS |
| -------------------------- | -------------- | ---------------- |
| Flexibility                | ✅ High         | 🚫 Lower         |
| Automatic Property Binding | 🚫             | ✅                |
| Custom Code Needed         | ✅              | 🚫               |
| Best for Microservices     | ✅              | ✅                |

---

If you need help setting up IAM permissions, creating secrets in AWS, or customizing property binding, feel free to reach out!
