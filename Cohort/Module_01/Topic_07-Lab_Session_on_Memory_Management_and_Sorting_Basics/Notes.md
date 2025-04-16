### Cost of removing an element

```
cost = sum of all the remaining elements in the array.
```

#### Formula, when have to remove all

```
Remove the largest first, and then next and so on .......
```

#### Example

###### A: 4 1 6

```
remove (6) --> 11 ----> (6 + 4 + 1)
remove (4) --> 5 ----> (4 + 1)
remove (1) --> 1 ----> (1)
```

#### Code

```java
cost (int[] arr){
	sort_desc(arr);
	
	int ans = 0;
	
	for (int i=0; i<n; i++){
		ans += arr[i] * (i+1);
	}
	
	return ans;
}

/*
	-------------------------
	TC -> O(nlogn)
	SC -> O(1)
	-------------------------
*/
```


### Nobel Integer
###### A[i] -> Nobel Integer, if no. of elements smaller than A[i] = A[i];

>Note: A is of distinct elements

	The following code will not work, if we have duplicate elements in the array.
```java
f(int[] arr){
	int cnt = 0;
	Arrays.sort(arr);
	
	for (int i=0; i<n; i++){
		if (arr[i] == i){
			cnt += 1;
		}
	}
	
	return cnt;
}

/*
	-------------------------
	TC -> O(nlogn)
	SC -> O(1)
	-------------------------
*/
```

### Nobel Integer Variation