## Q1. Search for a Range
**Problem Description**  

Given a **sorted array** of integers A (**0-indexed**) of size N, find the **left most** and the **right most** index of a given integer **B** in the array A.

Return an array of size 2, such that  
First element = Left most index of B in A  
Second element = Right most index of B in A.  
If B is not found in A, return [-1, -1].

**Note:** The time complexity of your algorithm must be O(log n).

**Problem Constraints**  

1 <= N <= 10^6  
1 <= A[i], B <= 10^9

**Input Format**  

The first argument given is the integer array A.  
The second argument given is the integer B.

**Output Format**  

Return the left most and right most index **(0-based)** of B in A as a 2-element array. If B is not found in A, return [-1, -1].

**Example Input**  

Input 1:

A = [5, 7, 7, 8, 8, 10]  
B = 8  

Input 2:

A = [5, 17, 100, 111]  
B = 3

**Example Output**  

Output 1:

[3, 4]  

Output 2:

[-1, -1]

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    // DO NOT MODIFY THE LIST. IT IS READ ONLY
    public ArrayList<Integer> searchRange(final List<Integer> A, int B) {
        ArrayList<Integer> res = new ArrayList<>();
        // Find the first and last occurrence of B in A
        int first = findOccurrence(A, B, true);
        int last = findOccurrence(A, B, false);
        res.add(first);
        res.add(last);
        return res;
    }

    private int findOccurrence(List<Integer> arr, int target, boolean isFirstOccurrence) {
        int n = arr.size();
        int start = 0, end = n - 1;
        int result = -1;

        while (start <= end) {
            int mid = start + (end - start) / 2;

            if (arr.get(mid) == target) {
                result = mid; // Potential result
                if (isFirstOccurrence) {
                    // Search on the left side for the first occurrence
                    end = mid - 1;
                } else {
                    // Search on the right side for the last occurrence
                    start = mid + 1;
                }
            } else if (arr.get(mid) < target) {
                start = mid + 1; // Move to the right half
            } else {
                end = mid - 1; // Move to the left half
            }
        }
        return result;
    }
}
```

## Q2. Square Root of Integer
**Problem Description**  

Given an integer **A**, compute and return the **square root of A**.  
If **A** is not a perfect square, return **floor(sqrt(A)).**

**NOTE:**   
The value of **A * A** can cross the range of Integer.  
Do not use the **sqrt function** from the standard library.  
Users are expected to solve this in **O(log(A))** time.

**Problem Constraints**  

0 <= A <= 10^9

**Input Format**  

The first and only argument given is the integer A.

**Output Format**  

Return floor(sqrt(A)).

**Example Input**  

Input 1:

11  

Input 2:

9

**Example Output**  

Output 1:

3  

Output 2:

3

```java
public class Solution {

    public int sqrt(int A) {

        if (A == 0 || A == 1) {
            return A;
        }

        long low = 1;
        long high = A/2;
        int res = 0;

        while (low <= high) {
            long mid = low + (high - low) / 2;
            long square = mid * mid;

            if (square == A) {
                return (int) mid;
            } else if (square < A) {
                res = (int) mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return res;
    }
}
```

>TC: (Log A) ---- Base 2

#### Why A/2A/2A/2 is appropriate:

- For a number A>1A > 1A>1, the square root of AAA is always less than or equal to A/2A / 2A/2 (this follows from basic mathematical properties).
    
- For example:
    
    - If A=4A = 4A=4, 4=2\sqrt{4} = 24​=2, and 2≤4/22 \leq 4/22≤4/2.
        
    - If A=9A = 9A=9, 9=3\sqrt{9} = 39​=3, and 3≤9/23 \leq 9/23≤9/2.
        

This helps reduce the search space for the binary search, improving efficiency.

#### Edge case for A=0A = 0A=0 or A=1A = 1A=1:

You’ve handled these explicitly, which is correct because:

- 0=0\sqrt{0} = 00​=0
    
- 1=1\sqrt{1} = 11​=1
## Q3. Sorted Insert Position
**Problem Description**

You are given a sorted array A of size N and a target value B.  
Your task is to find the index (0-based indexing) of the target value in the array.  

- If the target value is **present**, return its index.  
- If the target value is **not found**, return the index of the least element greater than or equal to B.  
- If the target value is **not found** and **least number greater than or equal to target is also not present**, return the length of the array _(i.e., the position where the target can be placed)._  

Your solution should have a time complexity of O(log(N)).

**Problem Constraints**

1 <= N <= 10^5  
1 <= A[i] <= 10^5  
1 <= B <= 10^5

**Input Format**

The first argument is an integer array A of size N.  
The second argument is an integer B.

**Output Format**

Return an integer denoting the index of the target value.

**Example Input**

Input 1:

A = [1, 3, 5, 6]  
B = 5

Input 2:

A = [1, 4, 9]  
B = 3

**Example Output**

Output 1:

2

Output 2:

1

**Example Explanation**

Explanation 1:

The target value is present at index 2.  

Explanation 2:

The target value should be inserted at index 1.

```java
public class Solution {

    public int searchInsert(ArrayList<Integer> A, int B) {

        int s = 0;
        int e = A.size() - 1;

        while (s <= e) {
            int mid = s + (e - s) / 2;
            if (A.get(mid) == B) {
                return mid;
            } else if (A.get(mid) > B) {
                e = mid - 1;
            } else {
                s = mid + 1;
            }
        }
        return s;
    }
}
```

## Q4. Find a Peak Element

**Problem Description**  

Given an **array** of integers **A**, find and return the **peak element** in it.  
An array element is considered a peak if it is not smaller than its neighbors. For corner elements, we need to consider only one neighbor.  

**NOTE:**

- It is guaranteed that the array contains only a single peak element.  
- Users are expected to solve this in O(log(N)) time. The array may contain duplicate elements.

**Problem Constraints**  

1 <= **|A|** <= 10^5  
1 <= A[i] <= 10^9

**Input Format**  

The only argument given is the integer array A.

**Output Format**  

Return the peak element.

**Example Input**  

Input 1:

A = [1, 2, 3, 4, 5]  

Input 2:

A = [5, 17, 100, 11]

**Example Output**  

Output 1:

5  

Output 2:

100

**Example Explanation**  

Explanation 1:

5 is the peak.  

Explanation 2:

100 is the peak.

```java
public class Solution {

    public int solve(ArrayList<Integer> A) {

        int s = 0;
        int e = A.size() - 1;

        while (s <= e) {
            int mid = s + (e - s) / 2;

            if ((mid == 0 || A.get(mid) >= A.get(mid - 1)) &&
                (mid == A.size() - 1 || A.get(mid) >= A.get(mid + 1))) {
                return A.get(mid);
            } else if (mid == A.size() - 1 || A.get(mid + 1) >= A.get(mid)) {
                s = mid + 1;
            } else {
                e = mid - 1;
            }
        }

        return -1;
    }
}
```

---
# Additional Problems
---
## Q.1 Matrix Search
**Problem Description**  
Given a matrix of integers **A** of size **N x M** and an integer **B**. Write an efficient algorithm that searches for integer **B** in matrix **A**.

This matrix A has the following properties:
1. Integers in each row are **sorted** from left to right.
2. The first integer of each row is greater than or equal to the last integer of the previous row.

Return **1** if **B** is present in **A**, else return **0**.
**NOTE:** Rows are numbered from top to bottom, and columns are from left to right.

**Problem Constraints**  
1 <= N, M <= 1000  
1 <= A[i][j], B <= 106
  
**Input Format**  
The first argument given is the integer matrix A.  
The second argument given is the integer B.
  
**Output Format**  
Return 1 if B is present in A else, return 0.
  
**Example Input**  
Input 1:

A = [ 
      [1,   3,  5,  7]
      [10, 11, 16, 20]
      [23, 30, 34, 50]  
    ]
B = 3

Input 2:

A = [   
      [5, 17, 100, 111]
      [119, 120, 127, 131]    
    ]
B = 3

  
  
**Example Output**  
Output 1:
1

Output 2:
0
  
**Example Explanation**  
Explanation 1:
 3 is present in the matrix at A[0][1] position so return 1.
Explanation 2:
 3 is not present in the matrix so return 0.
##### Code

```java
public class Solution {

    public int searchMatrix(ArrayList<ArrayList<Integer>> A, int B) {
        int N = A.size();
        int M = A.get(0).size();
        int i= 0;
        int j = M-1;

        while (i < N && j >= 0){
            if (A.get(i).get(j) == B){
                return 1;
            }
            else if (A.get(i).get(j) > B){
                j--;
            }
            else{
                i++;
            }
        }
        return 0;
    }
}
```

## Q2. Maximum height of staircase
**Problem Description**  
Given an integer **A** representing the number of square blocks. **The height of each square block is 1**. The task is to create a staircase of max-height using these blocks.
The first stair would require only one block, and the second stair would require two blocks, and so on.
Find and return the maximum height of the staircase.
  
**Problem Constraints**  
0 <= A <= 109

**Input Format**  
The only argument given is integer A.
  
**Output Format**  
Return the maximum height of the staircase using these blocks.
  
**Example Input**  
Input 1:
 A = 10

Input 2:
 A = 20
  
**Example Output**  
Output 1:
 4

Output 2:
 5
  
**Example Explanation**  
Explanation 1:
 The stairs formed will have height 1, 2, 3, 4.
Explanation 2:
 The stairs formed will have height 1, 2, 3, 4, 5.
##### Code
```java
public class Solution {

    public int solve(int A) {
        long low = 0;
        long high = A;
        int res = 0;

        while (low <= high){
            long mid = low + (high - low)/2;
            long sum = mid *(mid+1)/2;
            if (sum <= A){
                res = (int)mid;
                low = mid + 1;
            }
            else{
                high = mid -1;
            }
        }
        
        return res;
    }
}
```