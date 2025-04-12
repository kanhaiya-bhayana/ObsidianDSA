
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