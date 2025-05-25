## Q1. Rotated Sorted Array Search
**Problem Description**  

Given a sorted array of integers **A** of size **N** and an integer **B**,   
where array **A** is rotated at **some pivot** unknown beforehand.  
  
For example, the array [0, 1, 2, 4, 5, 6, 7] might become [4, 5, 6, 7, 0, 1, 2].  
  
Your task is to search for the target value B in the array. If **found**, return its **index**; **otherwise**, return **-1**.  
  
You can assume that **no duplicates** exist in the array.  
  
**NOTE:** You are expected to solve this problem with a time complexity of **O(log(N))**.
  
**Problem Constraints**  

1 <= N <= 1000000  
1 <= A[i] <= 109  
All elements in A are **Distinct**.
  
**Input Format**  

The First argument given is the integer array **A**.  
The Second argument given is the integer **B**.
  
**Output Format**  

Return index of B in array A, otherwise return **-1**
  
**Example Input**  

Input 1:

A = [4, 5, 6, 7, 0, 1, 2, 3]
B = 4 

Input 2:

A : [ 9, 10, 3, 5, 6, 8 ]
B : 5
  
**Example Output**  

Output 1:

 0 

Output 2:

 3

---

#### Solution 😀

```java
public class Solution {

    // DO NOT MODIFY THE LIST. IT IS READ ONLY
    public int search(final List<Integer> A, int B) {
        int n = A.size();
        int low = 0;
        int high = n - 1;

        while (low <= high){
            int mid = low + (high-low)/2;
            if (A.get(mid) == B){
                return mid;
            }
            if (A.get(low) <= A.get(mid)){ // left half is sorted
                if (A.get(low) <= B && A.get(mid) >= B){
                    high = mid - 1;
                }
                else{
                    low = mid + 1;
                }
            }
            else{ // right half is sorted
                if (A.get(mid) <= B && A.get(high) >= B){
                    low = mid + 1;
                }
                else{
                    high = mid - 1;
                }
            }
        }
        
        return -1;
    }
}
```

> TC: O(log N)
> SC: O(1)

---

## Q2. Single Element in Sorted Array
**Problem Description**  

Given a sorted array of integers **A** where every element appears twice except for one element which appears once, find and return this single element that appears only once.

Elements which are appearing twice are adjacent to each other.

**NOTE:** Users are expected to solve this in O(log(N)) time.
  
**Problem Constraints**  

1 <= **|A|** <= 100000

1 <= A[i] <= 10^9
  
**Input Format**  

The only argument given is the integer array A.
  
**Output Format**  

Return the single element that appears only once.

**Example Input**  

Input 1:

A = [1, 1, 7]

Input 2:

A = [2, 3, 3]
  
**Example Output**  

Output 1:

 7

Output 2:

 2
  
**Example Explanation**  

Explanation 1:

 7 appears once

Explanation 2:

 2 appears once

---

#### Solution 😀

```java
public class Solution {
    public int solve(ArrayList<Integer> A) {
        int low = 0, high = A.size() - 1;
        
        while (low < high) {
            int mid = low + (high - low) / 2;
            // Ensure mid is even so we can check the pair (mid, mid+1)
            if (mid % 2 == 1) mid--;
            if (A.get(mid).equals(A.get(mid + 1))) {
                // Pair is intact --> single element is after mid
                low = mid + 2;
            } else {
                // Pair is broken --> single element is at mid or before
                high = mid;
            }
        }
        
        // At the end, low == high --> pointing to the single element
        return A.get(low);
    }
}
```

#### ✅ Summary:

|Metric|Value|
|---|---|
|Time Complexity|O(log N)|
|Space Complexity|O(1)|
|Input Assumption|Sorted array, all elements appear twice except one|
|Example Input|`[1,1,2,2,3,4,4]` → `3`|

---
## Q3. Median of two sorted arrays

**Problem Description**  

Given two sorted arrays A and B of size M and N respectively, return the median of the two sorted arrays.  
Round of the value to the floor integer [2.6=2, 2.2=2]
  
**Problem Constraints**  

0 <= M <= 105  
0 <= N <= 105  
-109 <= A[i], B[i] <= 109
  
**Input Format**  

First argument A is an array of integers.  
First argument B is an array of integers.
  
**Output Format**  

Return an integer.
  
**Example Input**  

Input 1:

A = [5, 7]
B = [6]

Input 2:

A = [1, 2]
B = [3, 4]
  
**Example Output**  

Output 1:

6

Output 2:

2
  
**Example Explanation**  

Example 1:

merged array = [5, 6, 7] and median is 6.

Example 2:

merged array = [1, 2, 3, 4] and median is 
(2 + 3) / 2 = 2.5 
            = floor(2.5) 
            = 2



#### Solution 😀

```java
public class Solution {

    public int solve(ArrayList<Integer> A, ArrayList<Integer> B) {

        if (A.size() > B.size()){
            return solve(B,A);
        }


        int m = A.size();
        int n = B.size();

        int low = 0, high = m;
        int halfLen = (m + n + 1) /2;

        while (low <= high){
            int i = low + (high-low)/2;
            int j = halfLen - i;

            int maxLeft1 = (i==0) ? Integer.MIN_VALUE : A.get(i-1);
            int minRight1 = (i==m) ? Integer.MAX_VALUE : A.get(i);

            int maxLeft2 = (j==0) ? Integer.MIN_VALUE : B.get(j-1);
            int minRight2 = (j==n) ? Integer.MAX_VALUE : B.get(j);

            if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1){
                // if odd length of m + n
                if ((m+n)%2 != 0){
                    return Math.max(maxLeft1,maxLeft2);
                }

                else{ // when length is even
                    return (Math.max(maxLeft1, maxLeft2) + Math.min(minRight1,minRight2))/2;
                }
            }

            else if (maxLeft1 > minRight2){
                high = i - 1;
            }
            
            else{
                low = i + 1;
            }
        }

        return -1;
    }
}
```

#### ✅ Summary:

| Metric           | Value                      |
| ---------------- | -------------------------- |
| Time Complexity  | `O(log(min(m, n))`         |
| Space Complexity | `O(1)`                     |
| Approach         | Binary search partitioning |
| Handles Even/Odd | Yes                        |

---
# Additional Problems
---
## Q1. Matrix Median

**Problem Description**  

Given a matrix of integers **A** of size N x M in which each row is sorted.

Find and return the overall median of matrix A.

**NOTE**: No extra memory is allowed.

**NOTE**: Rows are numbered from top to bottom and columns are numbered from left to right.
  
**Problem Constraints**  

1 <= N, M <= 10^5

1 <= N*M <= 10^6

1 <= A[i] <= 10^9

N*M is odd
  
**Input Format**  

The first and only argument given is the integer matrix A.
  
**Output Format**  

Return the overall median of matrix A.

**Example Input**  

Input 1:

A = [   [1, 3, 5],
        [2, 6, 9],
        [3, 6, 9]   ] 

Input 2:

A = [   [5, 17, 100]    ]
  
**Example Output**  

Output 1:

 5 

Output 2:

 17

**Example Explanation**  

Explanation 1:

A = [1, 2, 3, 3, 5, 6, 6, 9, 9]
Median is 5. So, we return 5. 

Explanation 2:

Median is 17.

#### Solution 😀

```java
public class Solution {
    public int findMedian(ArrayList<ArrayList<Integer>> mat) {
        int rows = mat.size();
        int cols = mat.get(0).size();
        int low = Integer.MAX_VALUE;
        int high = Integer.MIN_VALUE;

        for (ArrayList<Integer> row : mat) {
            low = Math.min(low, row.get(0));
            high = Math.max(high, row.get(cols - 1));
        }

        int desired = (rows * cols + 1) / 2;

        while (low < high) {
            int mid = low + (high - low) / 2;
            int cnt = 0;

            // count elements <= mid using binary search
            for (ArrayList<Integer> row : mat) {
                cnt += countLessThanOrEqual(row, mid);
            }

            if (cnt < desired) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }

        return low;
    }

    private int countLessThanOrEqual(ArrayList<Integer> row, int target) {
        int low = 0, high = row.size();
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (row.get(mid) <= target) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low; // number of elements <= target
    }
}
```

### ✅ **Time Complexity**:

```
O(log(V) * R * log(C))
```

Where:

- `V` = Range of values in the matrix = (max element - min element)
    
- `R` = Number of rows
    
- `C` = Number of columns
    

**Explanation**:

- `log(V)` for binary search over the value range.
    
- `R * log(C)` to count how many elements ≤ mid (binary search in each row).
    

---

### ✅ **Space Complexity**:

```
O(1)
```

- No extra space used beyond a few variables.