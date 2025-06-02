# URL Shortener Design (Bitly)

**Reference:** NeetCodeIO  
**Source:** System Design School  

---

## Problem Statement

Design a URL shortener service, similar to Bitly, that allows users to generate and manage shortened URLs while providing efficient redirection and optional analytics.

---

## HTTP Redirect Codes

- **301 Redirect:** Permanent redirect (cached).
- **302 Redirect:** Temporary redirect (not cached).

---

## Steps to Approach the Design

1. **Gather Requirements**
2. **API and Database Design**
3. **High-Level Design**
4. **Detailed Design and Deep Dives**

---

## 1. Gather Requirements

### 1.1 Functional Requirements

1. **URL Shortening**  
   Generate a unique short URL for a given long URL.  
2. **URL Redirection**  
   Redirect users from a short URL to the corresponding long URL.  
3. **Link Analytics** *(Optional)*  
   Track the number of times each short URL is accessed.

### 1.2 Non-Functional Requirements

1. **Performance**  
   Minimize redirection latency to provide a seamless user experience.
2. **Scalability**  
   - Support **100 million daily active users (DAU)**.  
   - Handle **1 billion reads per day**, which translates to:  
     - **10,000 requests per second (RPS)**.
   - Store **1–5 billion URLs** over the lifetime of the service.

---

## Abbreviations

- **DAU:** Daily Active Users  
- **RPS:** Requests Per Second  
- **M:** Million  
- **B:** Billion  

---

## API Design

The API is structured to provide clear and concise endpoints for shortening and redirecting URLs. 

### 1. Shorten URL

**Endpoint:** `POST /api/urls/shorten`

- **Request Body:**  
  ```json
  {
    "longUrl": "https://example.com/long-url"
  }

* **Response Body:**

  ```json
  {
    "shortUrl": "https://bit.ly/abc123"
  }
  ```

---

### 2. Redirect from Short URL

**Endpoint:** `GET /api/urls/{shortUrl}`

* **Response:**

  * **301 Redirect:** When the result is cached.
  * **302 Redirect:** When the result is not cached.

> **Note:**
> The use of `302 Redirect` enables analytics by ensuring each request is processed by the server. This prevents caching, allowing accurate tracking and logging of user activity.

---

## Code Repository

Find the complete implementation in the repository:
[Bitly Clone Repository](https://dev.azure.com/Dev-user/J/_git/bitly)

---

## Additional Notes

* The document will be expanded with database schema and high-level design diagrams in the upcoming iterations.
* For contributions, fork the repository and submit a pull request.

