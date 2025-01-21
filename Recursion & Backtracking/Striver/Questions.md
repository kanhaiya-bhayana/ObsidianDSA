#### [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

```java
// TC: O(2^n) nearly, i.e., exponential 
class Solution {
    public int fib(int n) {
        return f(n);
    }

    private int f(int n){
        if (n <= 1)
            return n;

        int last = f(n-1);
        int sLast = f(n-2);

        return last + sLast;
    }
}
```

Optimization Solution is DP



## Subsequences
A contagious or non-contagious sequences, which follows the order. 
###### Example
```java
arr = {3, 1, 2}

3
1
2
3, 1
1, 2
3, 2
3, 1, 2
```

**Note**: Order has to be maintain!
##### Print all Subsequences - Popular Algorithm (Power Set)

Blueprint | Template for take and not take 
```java
// TC: O(2^n X n)
// SC: O(N)
public class Subsequence  
{  
    public void f(int ind,int []arr, List<Integer> ds)  
    {        // base case  
        if (ind >= arr.length)  
        {   printSubSeq(ds);  
            return;  
        }  
        ds.add(arr[ind]);  
        f(ind + 1, arr, ds); // take  
        ds.remove(ds.size()-1);  
        f(ind + 1, arr, ds); // not take  
    }  
  
    private void printSubSeq(List<Integer> ds) {  
        if (ds.isEmpty())  
        {            System.out.println("{}");  
            return;  
        }        for (int it : ds)  
        {            System.out.print(it +" ");  
        }        System.out.println();  
    }
}
```

