
### When we have circular nature

Suppose we have deque of circular nature, then update the front and rear using below formulas.

```
Increment --> rear = (rear + 1)%k; # k is the size of the dequeue
Decrement --> front = (front - 1 + k)%k;
```


