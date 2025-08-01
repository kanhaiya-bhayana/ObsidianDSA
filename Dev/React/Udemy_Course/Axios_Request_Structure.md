
 # #Axios
# 📡 Axios Request Structure

Axios is a promise-based HTTP client for the browser and Node.js. Below are common request patterns used in most frontend/backend applications.

---
## ✅ Basic Setup

First, install Axios (if not already):

```bash
npm install axios
```

Then import it in your file:

```javascript
import axios from 'axios';
```

---
## 📥 GET Request

```javascript
axios.get('https://api.example.com/data', {
  params: { id: 123 }, // Optional query parameters
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN' // Optional headers
  }
})
.then(response => {
  console.log(response.data);
})
.catch(error => {
  console.error('GET Error:', error);
});
```

---
## 📤 POST Request

```javascript
axios.post('https://api.example.com/data',
  {
    name: 'John Doe',
    age: 30
  },
  {
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer YOUR_TOKEN'
    }
  }
)
.then(response => {
  console.log(response.data);
})
.catch(error => {
  console.error('POST Error:', error);
});
```

---
## 🛠 PUT Request

```javascript
axios.put('https://api.example.com/data/123',
  {
    name: 'Updated Name'
  }
)
.then(response => {
  console.log(response.data);
})
.catch(error => {
  console.error('PUT Error:', error);
});
```

---
## ❌ DELETE Request

```javascript
axios.delete('https://api.example.com/data/123')
.then(response => {
  console.log('Deleted:', response.data);
})
.catch(error => {
  console.error('DELETE Error:', error);
});
```

---
## 🧱 Creating a Custom Axios Instance

Recommended for reusable configurations (e.g., base URL, headers):

```javascript
const api = axios.create({
  baseURL: 'https://api.example.com',
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN'
  }
});

// Usage
api.get('/data')
  .then(res => console.log(res.data))
  .catch(err => console.error(err));
```

---
## 📌 Notes

- Axios returns a promise; use `.then()` / `.catch()` or `async/await`.
    
- Use `params` for query strings and the second argument for body in `POST/PUT`.
    
- Always handle errors to avoid unhandled promise rejections.
