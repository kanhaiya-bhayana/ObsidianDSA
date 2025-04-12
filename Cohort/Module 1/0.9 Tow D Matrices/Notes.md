### 2D Matrices

Arrays of arrays

```java
int[][] mat = new int[N][M];
```

### Given a 2d matrix 

```
A[N][M] print the row-wise sum
```

```java
public void sumRow(int[][] arr){
	int n = arr.length;
	int m = arr[0].length;
	for (int i=0; i<n; i++){
		int sum = 0;
		for (int j=0; j<m; j++){
			sum += arr[i][j];
		}
		System.out.println(sum);
	}
}

/*
	-------------------------
	TC -> O(n*m)
	SC -> O(1)
	-------------------------
*/
```

### Given a 2D matrix 


```
A[N][M] print the column-wise sum
```

```java
public void sumCol(int[][] arr){
	int n = arr.length;
	int m = arr[0].length;
	for (int j=0; i<m; j++){
		int sum = 0;
		for (int i=0; i<m; i++){
			sum += arr[i][j];
		}
		System.out.println(sum);
	}
}

/*
	-------------------------
	TC -> O(n*m)
	SC -> O(1)
	-------------------------
*/
```


### Given a 2D matrix print diagonals

```java

// Pricipal Diagonal --> 1 5 9

/*
	1 2 3
  	4 5 6
  	7 8 9

*/


public void principalDiagonal(int[][] arr){
	int n = arr.length;
	int i=0;
	while (i < n){
		print(arr[i][i]);
		i +=1;
	}
}

/*
	-------------------------
	TC -> O(n)
	SC -> O(1)
	-------------------------
*/
```

```java

// Anti Diagonal --> 3 5 7

/*
	1 2 3
  	4 5 6
  	7 8 9

*/


public void antiDiagonal(int[][] arr){
	int n = arr.length;
	int i=0;
	while (i < n){
		print(arr[i][n-1-i]);
		i +=1;
	}
}

/*
	-------------------------
	TC -> O(n)
	SC -> O(1)
	-------------------------
*/
```


### Print diagonal in a rectangular matrix

```java
// half section code 
/*
	1 2  3  4
	5 6  7  8 
	9 10 11 12
*/
pubic void printDiagonalsRtoL(int[][] arr){
	int n = arr.length;
	int m = arr[0].length;
	
	int i = 0;
	for (int j=0; j<m; j++){
		int start = i;
		int end = j;
		while (start < N && end >= 0){
			print (A[start][end]);
			start += 1;
			end -= 1;
		}
		println();
	}
	
	int j = n-1;
}
```


### How many number of diagonals in a MxN matrix

```
M+N-1
```

### Print Anti Diagonals

```java
public void printAntiDiagonals(){
	List<List<Integer>> A = new ArrayList<>();
	A.add(new ArrayList<>(List.of(1,2,3)));
	A.add(new ArrayList<>(List.of(4,5,6)));
	A.add(new ArrayList<>(List.of(7,8,9)));
	
	ArrayList<ArrayList<Integer>> res = new ArrayList<>();

	int n = A.size();

	for (int i=0; i<2*n-1; i++){
		res.add(new ArrayList<>());
	}

	for (int i=0; i<n; i++){
		for (int j=0; j<n; j++){
			res.get(i+j).add(A.get(i).get(j));
		}
	}
	
	for (List<Integer> l : res){
		System.out.println(l);
	}
}

// TC -> O(n^2)
// SC -> O(n^2)
```

### Make all the elements in a row or column zero if the A i j = 0

```java
public class Solution {

    public ArrayList<ArrayList<Integer>> solve(ArrayList<ArrayList<Integer>> A) {

        int rows = A.size();
        int cols = A.get(0).size();

        // Arrays to mark which rows and columns need to be zeroed
        boolean[] row = new boolean[rows];
        boolean[] col = new boolean[cols];

        // Mark the rows and columns to be zeroed
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (A.get(i).get(j) == 0) {
                    row[i] = true;
                    col[j] = true;
                }
            }
        }

        // Update the matrix based on marked rows and columns
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (row[i] || col[j]) {
                    A.get(i).set(j, 0);
                }
            }
        }
        return A;
    }
} 

// Time Complexity: O(m × n)
// Space Complexity: O(m + n)
```


### Sum of all the main diagonal elements of **A**.

```java
public int diagonalSum(List<Integer> A){
	int n = A.size();
	
	int sum = 0;
	for (int i=0; i<n; i++){
		sum += A.get(i).get(i);
	}
	
	return sum;
}

// Time Complexity: O(n)
// Space Complexity: O(1)
```


### Minor Diagonal Sum

```java
public int solve(final List<ArrayList<Integer>> A){
	int n = A.size(); // Size of the matrix (N x N)
        int sum = 0;
        
        // Traverse the minor diagonal
        for (int i = 0; i < n; i++) {
            sum += A.get(i).get(n - i - 1);
        }
        
        return sum;
}
// Time Complexity: O(n)
// Space Complexity: O(1)
```


### Transpose a rectangle matrix

```java
public ArrayList<ArrayList<Integer>> solve(ArrayList<ArrayList<Integer>> A) {
	int rows = A.size();
	int cols = A.get(0).size();
	
	ArrayList<ArrayList<Integer>> res = new ArrayList<>();
	for (int i=0; i<cols; i++){
		res.add(new ArrayList<>());
	}

	for (int j=0; j<cols; j++){
		for (int i=0; i<rows; i++){
			res.get(j).add(A.get(i).get(j));
		}
	}

	return res;
}
// Time Complexity: O(m*n)
// Space Complexity: O(n*m)    

```

