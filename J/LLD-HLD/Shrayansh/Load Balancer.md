### **Load Balancing:**  

**Load balancing** is the process of distributing incoming traffic or workload across multiple servers or resources to ensure:  
- **High availability**,  
- **Efficient utilization**, and  
- **Scalability** of the system.

---

### **How It Works:**  
1. A **load balancer** acts as an intermediary between clients and backend servers.  
2. It routes client requests to one of the available servers based on a predefined strategy.  
3. If a server fails or is overloaded, the load balancer redirects traffic to healthy servers.  

---

### **Load Balancing Algorithms:**  
1. **Round Robin**: Requests are distributed sequentially to each server.  
2. **Least Connections**: Routes traffic to the server with the fewest active connections.  
3. **IP Hash**: Uses a hash of the client’s IP address to consistently route requests to the same server.  
4. **Weighted Round Robin**: Servers with higher weights receive more requests.  

---

### **Types of Load Balancers:**  
1. **Hardware Load Balancer**: Dedicated physical devices (e.g., F5, Citrix).  
2. **Software Load Balancer**: Applications like NGINX, HAProxy.  
3. **Cloud Load Balancer**: Managed services like AWS Elastic Load Balancer (ELB), Azure Load Balancer.

---

### **Benefits:**  
- **Improved Performance**: Prevents servers from becoming bottlenecks.  
- **High Availability**: Ensures continuous service during server failures.  
- **Scalability**: Supports horizontal scaling by adding more servers.  

---

### **Example Use Case:**  
In a web application, a load balancer distributes HTTP requests across multiple application servers to handle increased traffic efficiently.
