### Find the count of it's factors.

```java
public class Solution {
    public int solve(int A) {
        int cnt = 0;
        for (int i=1; i<=Math.sqrt(A); i++){
            if (A % i == 0){
                if (i == A/i){
                    cnt += 1;
                }
                else{
                    cnt += 2;
                }
            }
        }
        return cnt;
    }
}
```

### Return 1 if **A** is prime and return 0 if not

```java
public class Solution {
    public int solve(int A) {
        if (A <= 1) return 0;
        for (int i=2; i<=Math.sqrt(A); i++){
            if (A % i == 0){
                return 0;
            }
        }
        return 1;
    }
}
```