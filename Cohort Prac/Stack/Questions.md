## Stock Span Problem

```java
class StockSpanner {

    Stack<int[]> st;
    int span;
    public StockSpanner() {
        st = new Stack<>();
        span = -1;
    }
    
    public int next(int price) {
        span += 1;

        while (!st.isEmpty() && st.peek()[1] <= price){
            st.pop();
        }

        if (st.isEmpty()){
            st.push(new int[]{span, price});
            return span+1;
        }
        else{
            int res = st.peek()[0];
            st.push(new int[]{span, price});

            return span - res;
        }
    }
}
```

#### Summary
```
Time Complexity:

Amortized O(1) per next call.

O(n) for 𝑛 calls.

Space Complexity:

O(n) (for the stack).
```

