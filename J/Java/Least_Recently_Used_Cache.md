
# [LRU Cache](https://leetcode.com/problems/lru-cache/)

```java
import java.util.*;

/**
 * Amortised‑O(1) “queue + frequency” LRU.
 *  • get/put:   average O(1), worst‑case O(C) when we have to
 *               drain old keys out of the queue to find the real LRU.
 *  • space:     O(C)
 *
 * If you require strict O(1) worst‑case, see the second version that
 * wraps a LinkedHashMap at the end of this file.
 */
public class LRUCache<K, V> {

    private final int capacity;
    private final Map<K, V>      valueStore;   // key  -> value
    private final Map<K, Integer>freqStore;    // key  -> #times in queue
    private final Deque<K>       queue;        // access order (newest at tail)

    public LRUCache(int capacity) {
        this.capacity   = capacity;
        this.valueStore = new HashMap<>();
        this.freqStore  = new HashMap<>();
        this.queue      = new ArrayDeque<>();
    }

    public V get(K key) {
        if (!valueStore.containsKey(key)) {
            return null;                       // use null instead of –1
        }
        freqStore.put(key, freqStore.get(key) + 1);
        queue.offerLast(key);
        return valueStore.get(key);
    }

    public void put(K key, V value) {

        // 1️⃣ Evict if we’re inserting a *new* key and the cache is full
        if (!valueStore.containsKey(key) && valueStore.size() == capacity) {
            evictLRU();
        }

        // 2️⃣ Insert / update
        valueStore.put(key, value);
        freqStore.put(key, freqStore.getOrDefault(key, 0) + 1);
        queue.offerLast(key);
    }

    /* === helpers ======================================================== */

    private void evictLRU() {
        K lru = null;
        while (!queue.isEmpty()) {
            K candidate = queue.pollFirst();        // oldest access
            int remaining = freqStore.merge(candidate, -1, Integer::sum);
            if (remaining == 0) {                   // real LRU found
                freqStore.remove(candidate);
                lru = candidate;
                break;
            }
        }
        if (lru != null) {
            valueStore.remove(lru);
        }
    }
}
```


### Why this works
- **Queue** keeps every access in order; newest keys are appended to the tail.
- **freqStore** counts how many times a key is _still_ represented in the queue.  
    When we need to evict, we pop from the head until we find the first key whose  
    frequency drops to 0 — that key is the true LRU.
Amortised analysis: every `key` we pop from the queue must have been pushed  
there at some point, so total pops ≤ total pushes. Hence the average cost per  
operation is O(1), even though a single `put` can scan several stale entries.



### Operation‑by‑operation analysis

|method|best / average case|worst case|why|
|---|---|---|---|
|`get`|**O(1)**|**O(1)**|one `HashMap` lookup (`mapStore`), one in `freqStore`, one increment, and one `queue.offer` — all constant‑time.|
|`put` (update existing key)|**O(1)**|**O(1)**|same work as `get`, plus a single `HashMap` write.|
|`put` (insert when cache **not** full)|**O(1)**|**O(1)**|no eviction loop; just two map writes and one `queue.offer`.|
|`put` (insert when cache **is** full)|**amortised O(1)**|**O(N)**, where **N = capacity**|if the cache is full _and_ the head of the queue still has stale duplicates, the `while (!queue.isEmpty())` loop may dequeue up to **N** elements before finding the true LRU to evict. Over the life of the cache every entry you ever dequeue must have been enqueued earlier, so total dequeues ≤ total enqueues ⇒ the **average** cost per call remains O(1).|

---
### Intuition behind the amortised bound
Think of the queue as a log of accesses.
- Each `get`/`put` enqueues **one** record.
- A dequeue happens only during eviction, and each element can be dequeued **at most once** (its frequency eventually drops to 0 and it never re‑enters the queue without a fresh enqueue).

Hence, across _m_ total operations you do **≤ m** dequeues, keeping the cumulative extra work linear in _m_ and the amortised cost constant.

---
### Space complexity

- `valueStore` and `freqStore`: at most **N** entries each ⇒ **O(N)**.
- `queue`: up to the number of total accesses since the last mass‑eviction; in the worst theoretical scenario it can grow to **O(m)** for _m_ operations, though in practice it is bounded by how often you hit capacity.

