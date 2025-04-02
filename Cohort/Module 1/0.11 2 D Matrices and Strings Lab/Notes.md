### Transpose a matrix

##### Observation

- Principle diagonals are same, 
	- just do the swap with it counter parts

```
------------------
Matrix 3x3
------------------

1 2 3 
4 5 6 
7 8 9

------------------
Transpose
------------------

1 4 7
2 5 8
3 6 9

Swaps
-------------------
(0,1)  <-->  (1,0)
(0,2)  <-->  (2,0)
(0,3)  <-->  (3,0)
```

##### Code

```java
public int[][] transposeMatrix(int[][] mat){
	int rows = mat.length;
	int cols = mat[0].length;
	
	for (int i=0; i<rows; i++){
		for (int j=i+1; j<cols; j++){
			int t = mat[i][j];
			mat[i][j] = mat[j][i];
			mat[j][i] = t;
		}
	}
	
	return mat;
}

// Time Complexity: O(n^2)
// Space Complexity: O(1)
```

### Rotate the matrix by **90** degrees (clockwise).

###### 2 Step Process
1. Transpose a matrix
	1. Principle diagonals are same,
	2. Do the swap with the counter parts
2. Reverse each row
```java
public int[][] transposeMatrix(int[][] mat){
	int rows = mat.length;
	int cols = mat[0].length;
	
	// step 1
	for (int i=0; i<rows; i++){
		for (int j=i+1; j<cols; j++){
			int t = mat[i][j];
			mat[i][j] = mat[j][i];
			mat[j][i] = t;
		}
	}
	
	// step 2
	for (int[] row : mat){
		int i = 0;
		int j = cols - 1;
		while (i < j){
			int t = row[i];
			row[i] = row[j];
			row[j] = t;
			i++;
			j--;
		}
	}
	
	return mat;
}

// Time Complexity: O(n^2)
// Space Complexity: O(1)
```


### Given an arrays of 1s and 0s, you are allowed to replace a 0 with a 1. Find the maximum number of consecutive 1s that can be obtained after the initial replacement

##### Asked By Companies
```
Amazon, Microsoft, Adobe
```

##### **Algorithm (Step-by-Step) for Finding Maximum Consecutive `1`s After Replacing One `0`**

---

##### **Approach: Using Left and Right Count Arrays (O(n) Space)**

1. **Initialize variables:**
    
    - `n = arr.length`
        
    - Arrays `leftOnes[n]` and `rightOnes[n]` to store consecutive `1`s on the left and right of each `0`.
        
    - `maxLen = 0`, `count = 0`, `zeroCount = 0`.
        
2. **Compute left contiguous ones count (`leftOnes`):**
    
    - Traverse from `0 → n-1`:
        
        - If `arr[i] == 1`, increment `count` and store `-1` in `leftOnes[i]`.
            
        - If `arr[i] == 0`, store `count` in `leftOnes[i]`, reset `count`, and increment `zeroCount`.
            
3. **Compute right contiguous ones count (`rightOnes`):**
    
    - Traverse from `n-1 → 0`:
        
        - If `arr[i] == 1`, increment `count` and store `-1` in `rightOnes[i]`.
            
        - If `arr[i] == 0`, store `count` in `rightOnes[i]`, reset `count`.
            
4. **Handle edge case:**
    
    - If `zeroCount == 0`, return `n` (since no `0` exists, flipping is not required).
        
5. **Find maximum consecutive `1`s after flipping one `0`:**
    
    - Traverse from `0 → n-1`:
        
        - If `arr[i] == 0`, calculate:
            
            ```
            left = (leftOnes[i] == -1) ? 0 : leftOnes[i]
            right = (rightOnes[i] == -1) ? 0 : rightOnes[i]
            maxLen = max(maxLen, left + right + 1)
            ```
            
    - Return `maxLen`.
        

---
##### Code

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {1, 1, 1, 1, 1};  // All 1s edge case
        System.out.println("Max Consecutive 1s after replacement: " + maxConsecutiveOnes(arr));
    }

    public static int maxConsecutiveOnes(int[] arr) {
        int n = arr.length;
        int[] leftOnes = new int[n];
        int[] rightOnes = new int[n];
        int maxLen = 0, count = 0, zeroCount = 0;

        // Compute left contiguous ones count
        for (int i = 0; i < n; i++) {
            if (arr[i] == 1) {
                count++;
                leftOnes[i] = -1;
            } else {
                leftOnes[i] = count;
                count = 0;
                zeroCount++;  // Count zeros
            }
        }

        count = 0;
        // Compute right contiguous ones count
        for (int i = n - 1; i >= 0; i--) {
            if (arr[i] == 1) {
                count++;
                rightOnes[i] = -1;
            } else {
                rightOnes[i] = count;
                count = 0;
            }
        }

        // If no zero exists, return full array length
        if (zeroCount == 0) return n;

        // Compute max length by replacing one zero
        for (int i = 0; i < n; i++) {
            if (arr[i] == 0) {
                int left = (leftOnes[i] == -1) ? 0 : leftOnes[i];
                int right = (rightOnes[i] == -1) ? 0 : rightOnes[i];
                maxLen = Math.max(maxLen, left + right + 1);
            }
        }

        return maxLen;
    }
}

// Time Complexity: O(n)
// Space Complexity: O(n)
```