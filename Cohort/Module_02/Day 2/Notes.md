
## Given a 2D array of sorted rows and cols, find the element k present or not

### Brute Force

```
TC: O(NxM) 
SC: O(1)
```

#### Approach 2 - Exploiting the sorted rows and/or columns

```
rather than starting with (0,0), it would be great if you start with top right, and compare the element k if curr > k then that will definitly greater than all of the values in the column, then jsut do col--, and like that.
```

```
We have two equal good option to start:
	1. Top right (0, M-1)
	2. Bottom left (N-1, 0)
```

### Pseudo Code
```java
public boolean checkPresent(int[][] mat, int k){
	int N = mat.length;
	int M = mat[0].length;
	int i=0;
	int j=M-1;
	while (i < N && j >=0){
		if (mat[i][j] == k){
			return true;
		}
		if (mat[i][j] > k){
			j--; // skip column;
		}
		else {
			i++; // skip row
		}
	}
	return false;
}


// TC: O(N+M) since every step we are discarding a rwo or a column
// SC: O(1)
```

---
## SubMatrix
Same as,
	Continuous part of an array is subarry.
	a submatrix is a continuous part of a matrix


#### #Goolge #Facebook #Submatrix
### Given a matrix of N rows and M cols, determine the sum of all the submatrices

##### How many submatrix a matrix could have?
>  No. of subarrays with N rows x No. of subarrays with M cols = Total submatrices
>  N*(N+1)/2 x M*(M+1)/2

#### Approach 1

> Consider all the submatrices and find the sum of all.

```
TC: O(N^3 * M^3)
SC: o(1)
```

#### Approach 2

To find the sum of all submatrices of a given matrix in Java, we can calculate the contribution of each element to the sum of all submatrices. The key idea is to determine how many submatrices include each element and then multiply the element's value by this count.

Here's the Java code to accomplish this:

```java
public class SumOfAllSubmatrices {
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2},
            {3, 4}
        };

        int sum = sumOfAllSubmatrices(matrix);
        System.out.println("Sum of all submatrices: " + sum);
    }

    public static int sumOfAllSubmatrices(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int totalSum = 0;

        // Iterate through each element in the matrix
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // Calculate the number of submatrices including matrix[i][j]
                int topLeft = (i + 1) * (j + 1);
                int bottomRight = (rows - i) * (cols - j);

                // Contribution of matrix[i][j] to the total sum
                totalSum += matrix[i][j] * topLeft * bottomRight;
            }
        }

        return totalSum;
    }
}
```

### Explanation

1. **Contribution of Each Element**:
    
    - For an element at position `(i, j)`, the number of submatrices containing it is calculated as:
        
        - `topLeft = (i + 1) * (j + 1)` → Number of ways to choose the top-left corner.
            
        - `bottomRight = (rows - i) * (cols - j)` → Number of ways to choose the bottom-right corner.
            
2. **Total Contribution**:
    
    - Multiply the element's value by the product of `topLeft` and `bottomRight` to find its total contribution.
        
3. **Iterate Over the Matrix**:
    
    - Sum the contributions of all elements to get the total sum.
        

### Example

For the matrix:

```
1 2
3 4
```

- The sum of all submatrices is:
    

```
(1 * 1*1 * 2*2) + (2 * 1*2 * 2*1) + (3 * 2*1 * 1*2) + (4 * 2*2 * 1*1)
= 20 + 16 + 12 + 16
= 64
```

Output:

```
Sum of all submatrices: 64
```
### Final Complexity Analysis

- **Time Complexity**: O(N×M)O(N \times M) — Iterates over every element of the matrix once.
    
- **Space Complexity**: O(1)O(1) — No additional space is used beyond the input matrix and a few scalar variables.