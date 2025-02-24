### **Proxy:**

A **proxy** is an intermediary server or service that acts as a bridge between clients and backend servers.

### Types of Proxies:

1. **Forward Proxy**:
    
    - Used by clients to access external servers.
    - Example: Accessing a blocked website via a proxy.
2. **Reverse Proxy**:
    
    - Sits in front of backend servers to handle client requests.
    - Example: Load balancers (e.g., Nginx, HAProxy).

### Use Cases:

1. **Security**: Hides the IP address of clients or servers.
2. **Load Balancing**: Distributes traffic across multiple servers.
3. **Caching**: Stores frequently accessed content to improve performance.
4. **Access Control**: Filters traffic based on rules.

### Example:

A reverse proxy like Nginx handles incoming requests, routes them to backend microservices, and forwards the response to the client.