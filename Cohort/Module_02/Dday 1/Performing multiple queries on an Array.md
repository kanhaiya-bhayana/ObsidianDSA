### Given an integer array A where every element is 0, return a final array after performing multiple queries.

### Query (i,x): Add x to all the numbers from i to N - 1

### EX:

```
N = 7, A = [0 0 0 0 0 0 0]
Query (1,3)
Query (4,-2)
Query (3,1)
```


#### Approach 1 #Approach1
Loop for each query and perform query operation

```
TC: O(Q*N)
SC: O(1)
```

##### Code

```java
func(int[] arr, int[][] query){
	int[] arr = {0,0,0,0,0,0,0};
		
	int[][] query = {
		{1,3},{4,-2},{3,1}
	};
	
	int n = arr.length;
	for (int[] q : query){
		int i = q[0];
		int v = q[1];
		
		while (i < n){
			arr[i] += v;
			i++;
		}
	}
}
```

#### Approach 2 - Lazy Sum

Try to avoid traversing till the end for each query.
	-> Add x at arr[i] for each query
	-> Take prefix once at the end



```
TC: O(Q+N)
SC: O(1)
```

```java
public static void main(String[] args) {
    System.out.println("Hello World");
    int[] arr = {0, 0, 0, 0, 0, 0, 0};
    
    int[][] qr = {
        {1, 3}, {4, -2}, {3, 1}
    };
    
    int n = arr.length;
    for (int[] q : qr) {
        int i = q[0];
        int v = q[1];
        
        if (i < n) { // Ensure the index is within bounds
            arr[i] += v;
        }
    }

    for (int i = 1; i < n; i++) {
        arr[i] += arr[i - 1]; // Cumulative summation
    }
    
    for (int i : arr) {
        System.out.print(i + ", ");
    }
}

```


---

## Modified Problem
### Given an integer array A where every element is 0, return a final array after performing multiple queries.

### Query (i,j,x): Add x to all the numbers from i to j where i <= j

### EX:

```
N = 7, A = [0 0 0 0 0 0 0]
Query (1,3,2)
Query (2,5,3)
Query (5,6,-1)
```

#### Same as approach 1 of above problem #Approach1 

#### Approach 2

Everything is same as the above question, you only need to take care of the following formula: 

> Query(i, j, x) = Query (i, x) + Query(j + 1, -x)

```
TC: O(Q+N)
SC: O(1)
```

```java
public static void main(String[] args) {
		System.out.println("Hello World");
		int[] arr = {0,0,0,0,0,0,0};
		
		int[][] qr = {
		    {1,3,2},{2,5,3},{5,6,-1}
		};
		
		int n = arr.length;
		for (int[] q : qr){
		    int i = q[0];
		    int j = q[1];
		    int v = q[2];
		    if (i < n){
		        arr[i] += v;
		    }
		    if (j+1 < n){
		        arr[j+1] -= v;
		    }
		}

		for (int i=1; i<n; i++){
		    arr[i] += arr[i-1];
		}
		
		for (int i : arr){
		    System.out.print(i + ", ");
		}
	}
```

## Follow up question

### Everything is same as the above question but the array Array elements are non-zero