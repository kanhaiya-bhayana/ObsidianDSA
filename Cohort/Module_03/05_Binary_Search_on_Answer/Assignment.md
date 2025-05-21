## Q1. Painter's Partition Problem
**Problem Description**

Given 2 integers **A** and **B** and an array of integers **C** of size **N**. Element **C[i]** represents the length of **ith** board.  
You have to paint all **N** boards **[C0, C1, C2, C3 … CN-1]**. There are **A** painters available and each of them takes **B** units of time to paint **1 unit** of the board.

Calculate and return the minimum time required to paint all boards under the constraints that **any painter will only paint contiguous sections of the board.**

**NOTE:**  
1. 2 painters cannot share a board to paint. That is to say, a board cannot be painted partially by one painter, and partially by another.  
2. A painter will only paint contiguous boards. This means a configuration where painter 1 paints boards 1 and 3 but not 2 is invalid.  

Return the **ans % 10000003**.

---

**Problem Constraints**

1 <= A <= 1000  
1 <= B <= 10⁶  
1 <= N <= 10⁵  
1 <= C[i] <= 10⁶

---

**Input Format**

The first argument given is the integer A.  
The second argument given is the integer B.  
The third argument given is the integer array C.

---

**Output Format**

Return minimum time required to paint all boards under the constraints that any painter will only paint contiguous sections of board % 10000003.

---

**Example Input**

Input 1:
A = 2  
B = 5  
C = [1, 10]

Input 2:

A = 10  
B = 1  
C = [1, 8, 11, 3]

**Example Output**

Output 1:
```
50
```

Output 2:
```
11
```
#### Solution 🙂

```java
public class Solution {

    public int paint(int A, int B, ArrayList<Integer> C) {
        int mod = 10000003;

        long maxLen = 0;
        long sum = 0;

        for (int n : C) {
            maxLen = Math.max(n, maxLen);
            sum += n;
        }

        long low = maxLen;
        long high = sum;
        long res = 0;

        while (low <= high) {
            long mid = low + (high - low) / 2;

            if (isPossible(C, A, mid)) {
                res = mid;
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }

        return (int) ((res * B) % mod);
    }

    private boolean isPossible(ArrayList<Integer> arr, int painters, long maxTime) {
        long total = 0;
        int cnt = 1; // Start with 1 painter

        for (int a : arr) {
            total += a;

            if (total > maxTime) {
                cnt++;
                total = a;

                if (cnt > painters) {
                    return false;
                }
            }
        }

        return true;
    }
}
````

---

## Q2. Aggressive cows

**Problem Description**  

Farmer John has built a new long barn with **N** stalls. Given an array of integers **A** of size **N** where each element of the array represents the location of the stall and an integer **B** which represents the number of cows.

His cows don't like this barn layout and become aggressive towards each other once put into a stall. To prevent the cows from hurting each other, John wants to assign the cows to the stalls, such that the **minimum distance** between any two of them is as large as possible. What is the **largest minimum distance**?
  
**Problem Constraints**  

2 <= N <= 100000  
0 <= A[i] <= 109  
2 <= B <= N
  
**Input Format**  

The first argument given is the integer array **A**.  
The second argument given is the integer **B**.
  
**Output Format**  

Return the **largest minimum distance** possible among the cows.
  
**Example Input**  

Input 1:

A = [1, 2, 3, 4, 5]
B = 3

Input 2:

A = [1, 2]
B = 2
  
**Example Output**  

Output 1:

 2

Output 2:

 1

---

#### Solution 🙂

```java
public class Solution {

    public int solve(ArrayList<Integer> A, int B) {
        Collections.sort(A);
        int low = 1;
        int high = A.get(A.size()-1) - A.get(0);
        int res = 0;

        while (low <= high){
            int mid = low + (high - low)/2;
            if (canPlaceCows(A,B,mid)){
                res = mid;
                low = mid + 1;
            }
            else{
                high = mid - 1;
            }
        }

        return res;
    }

    private boolean canPlaceCows(ArrayList<Integer> arr, int cows, int minDist){
        int cnt = 1;
        int lastPos = arr.get(0);
        
        for (int i=1; i<arr.size(); i++){
            if (arr.get(i) - lastPos >= minDist){
                cnt++;
                lastPos = arr.get(i);
                if (cnt == cows){
                    return true;
                }
            }
        }
        
        return false;
    }
}
```

---

