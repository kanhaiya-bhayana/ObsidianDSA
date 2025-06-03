## Q1. Largest Rectangle in Histogram

**Problem Description**  
Given an array of integers **A**.

**A** represents a histogram i.e **A[i]** denotes the height of the **ith** histogram's bar. Width of each bar is **1**.

Find the area of the largest rectangle formed by the histogram.
  
**Problem Constraints**  

1 <= |A| <= 100000

1 <= A[i] <= 10000

**Input Format**  

The only argument given is the integer array A.
  
**Output Format**  

Return the area of the largest rectangle in the histogram.

**Example Input**  

Input 1:

 A = [2, 1, 5, 6, 2, 3]

Input 2:

 A = [2]
  
**Example Output**  

Output 1:

 10

Output 2:

 2
  
**Example Explanation**  

Explanation 1:

The largest rectangle has area = 10 unit. Formed by A[3] to A[4].

Explanation 2:

Largest rectangle has area 2.
  

Expected Output

Provide sample input and click run to see the correct output for the provided input. Use this to improve your problem understanding and test edge cases

##### Code
```java
public class Solution {

    public int largestRectangleArea(ArrayList<Integer> A) {
        return largestRectangleArea(A.stream().mapToInt(i -> i).toArray());
    }

    public int largestRectangleArea(int[] heights) {
        if (heights.length == 1){
            return heights[0];
        }

        int n = heights.length;
        int[] pse = previousSmallerIndices(heights);
        int[] nse = nextSmallerIndices(heights);
        int max = -1;

        for (int i=0; i<n; i++){
            int width = (nse[i] - pse[i] - 1);
            heights[i] = width * heights[i];
            max = Math.max(max,heights[i]);
        }
        return max;
    }

  

    public int[] previousSmallerIndices(int[] arr) {
        int n = arr.length;
        int[] pse = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }
            pse[i] = st.isEmpty() ? -1 : st.peek();
            st.push(i);
        }
        return pse;
    }
  
    public int[] nextSmallerIndices(int[] arr) {
        int n = arr.length;
        int[] nse = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }
            nse[i] = st.isEmpty() ? n : st.peek();
            st.push(i);
        }
        return nse;
    }
}
```

## Q2. Double Character Trouble

**Problem Description**  

You have a string, denoted as **A**.  
  
To transform the string, you should perform the following operation repeatedly:  

1. Identify the **first occurrence** of **consecutive identical pairs** of characters within the string.
2. **Remove this pair** of identical characters from the string.
3. Repeat steps 1 and 2 until there are **no more** consecutive identical pairs of characters.

The **final result** will be the transformed string.
  
**Problem Constraints**  

1 <= |A| <= 100000
  
**Input Format**  

First and only argument is string A.
  
**Output Format**  

Return the final string.
  
**Example Input**  

Input 1:

 A = "abccbc"

Input 2:

 A = "ab"

**Example Output**  

Output 1:

 "ac"

Output 2:

 "ab"
  
**Example Explanation**  

Explanation 1:

The Given string is "**abccbc**".  
  
Remove the first occurrence of consecutive identical pairs of characters "**cc**".  
After removing the string will be "**abbc**".  
  
Again Removing the first occurrence of consecutive identical pairs of characters "**bb**".  
After remvoing, the string will be "**ac**".  
  
Now, there is **no** consecutive identical pairs of characters.  
Therefore the string after this operation will be "**ac**".

Explanation 2:

 **No removals** are to be done.

  ##### Code
```java
public class Solution {
    public String solve(String s) {
        int n = s.length();
        Stack<Character> st = new Stack<>();
        for (char c : s.toCharArray()){
            if (!st.isEmpty() && st.peek() == c){
                st.pop();
            }else{
                st.push(c);
            }
        }

        StringBuilder res = new StringBuilder();
        while (!st.isEmpty()){
            res.append(st.pop());
        }
        return res.reverse().toString();
    }
}  
```


## Q3. MAX and MIN

**Problem Description**  

Given an array of integers **A**.

The value of an array is computed as the difference between the **maximum** element in the array and the **minimum** element in the array **A**.

Calculate and return the sum of values of all possible subarrays of A **modulo 109+7**.
  
**Problem Constraints**  

1 <= |A| <= 100000

1 <= A[i] <= 1000000
  
**Input Format**  

The first and only argument given is the integer array A.
  
**Output Format**  

Return the sum of values of all possible subarrays of A modulo 109+7.
  
**Example Input**  

Input 1:

 A = [1]

Input 2:

 A = [4, 7, 3, 8]

**Example Output**  

Output 1:

 0

Output 2:

 26
  
**Example Explanation**  

Explanation 1:

Only 1 subarray exists. Its value is 0.

Explanation 2:

value ( [4] ) = 4 - 4 = 0
value ( [7] ) = 7 - 7 = 0
value ( [3] ) = 3 - 3 = 0
value ( [8] ) = 8 - 8 = 0
value ( [4, 7] ) = 7 - 4 = 3
value ( [7, 3] ) = 7 - 3 = 4
value ( [3, 8] ) = 8 - 3 = 5
value ( [4, 7, 3] ) = 7 - 3 = 4
value ( [7, 3, 8] ) = 8 - 3 = 5
value ( [4, 7, 3, 8] ) = 8 - 3 = 5
sum of values % 10^9+7 = 26


##### Code
```java
public class Solution {

    public int solve(ArrayList<Integer> A) {

        final int MOD = 1_000_000_007;
        long ans = 0;
        int n = A.size();
        int[] arr = A.stream().mapToInt(i -> i).toArray();

        int[] NGER = nextGreaterElement(arr);
        int[] NGEL = previousGreaterElement(arr);
        int[] NSEL = previousSmallerElement(arr);
        int[] NSER = nextSmallerElement(arr);

  

        for (int i = 0; i < n; i++) {
            long cntMax = (long)(i - NGEL[i]) * (NGER[i] - i) % MOD;
            long cntMin = (long)(i - NSEL[i]) * (NSER[i] - i) % MOD;
            long contrib = ((cntMax - cntMin + MOD) % MOD) * A.get(i) % MOD;
            ans = (ans + contrib) % MOD;
        }

        return (int) ans;
    }

    public int[] previousSmallerElement(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }
            result[i] = st.isEmpty() ? -1 : st.peek();
            st.push(i);
        }
        return result;
    }

    public int[] previousGreaterElement(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && arr[st.peek()] <= arr[i]) {
                st.pop();
            }
            result[i] = st.isEmpty() ? -1 : st.peek();
            st.push(i);
        }
        return result;
    }
  
    public int[] nextSmallerElement(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }
            result[i] = st.isEmpty() ? n : st.peek(); // n for out-of-bound
            st.push(i);
        }
        return result;
    }
  
    public int[] nextGreaterElement(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && arr[st.peek()] <= arr[i]) {
                st.pop();
            }
            result[i] = st.isEmpty() ? n : st.peek(); // n for out-of-bound
            st.push(i);
        }
        return result;
    }
}
```


