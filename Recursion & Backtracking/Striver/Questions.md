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


## All SubSequences where sum = k
```java
import java.util.*;

/**
 * This class finds all possible subsequences in an array whose elements sum up to a target value.
 * It uses backtracking to generate all possible combinations.
 */
public class SubsequenceWithSum {
    // Input array to process
    private static int[] numbers;
    // Target sum we're looking for
    private static int targetSum;
    // List to store all valid subsequences
    private static List<List<Integer>> validSubsequences;

    public static void main(String[] args) {
        // Initialize input values
        numbers = new int[]{1, 2, 1};
        targetSum = 2;
        validSubsequences = new ArrayList<>();

        // Start the recursive backtracking process
        findSubsequences(0, new ArrayList<>(), 0);

        // Print all found subsequences
        for (List<Integer> subsequence : validSubsequences) {
            System.out.println(subsequence);
        }
    }

    /**
     * Recursive method to find all subsequences with the target sum.
     * Uses backtracking to explore all possible combinations.
     *
     * @param index Current index in the array being processed
     * @param currentSubsequence List containing current subsequence being built
     * @param currentSum Running sum of elements in current subsequence
     */
    private static void findSubsequences(int index, List<Integer> currentSubsequence, int currentSum) {
        // Base case: reached end of array
        if (index == numbers.length) {
            // If current sum equals target sum, add subsequence to results
            if (currentSum == targetSum) {
                validSubsequences.add(new ArrayList<>(currentSubsequence));
            }
            return;
        }

        // Include current element in subsequence
        currentSubsequence.add(numbers[index]);
        currentSum += numbers[index];
        findSubsequences(index + 1, currentSubsequence, currentSum);

        // Backtrack: exclude current element from subsequence
        currentSubsequence.remove(currentSubsequence.size() - 1);
        currentSum -= numbers[index];
        findSubsequences(index + 1, currentSubsequence, currentSum);
    }
}
```
