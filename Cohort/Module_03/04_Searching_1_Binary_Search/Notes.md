### #BinarySearch
### Q. Given a sorted array with integers search & find the inedex of a given element K If not present, return -1

##### Code
```java
public int binarySearch(int[] arr, int K){
	int s = 0;
	int e = arr.length - 1;
	
	while (s <= e){
		int mid = s + (e-s) /2 ;
		if (arr[mid] == K){
			return mid;
		}
		else if (arr[mid] > K){
			e = mid -1;
		}
		else{
			s = mid + 1;
		}
	}
	
	return -1;
}
```

> TC: O(Log n)
> SC: O(1)

### #BinarySearch
### Q. Given a sorted array of size N with duplicates. Find the index of first occurrence of a given target.

##### Code
```java
public int search(int[] arr, int k){
	int s = 0;
	int e = arr.length-1;
	
	while (s <= e){
		int mid = s + (e-s)/2;
		if (arr[mid] == k){
			if (mid == 0 || arr[mid-1] != arr[mid]){
				return mid;
			}
			else{
				e = mid - 1;
			}
		}
		else if (arr[mid] > k){
			e = mid - 1;
		}
		else{
			s = mid + 1;
		}
	}
	return -1;
}
```

>TC: O(Log n)
>SC: O(1)

### #BinarySearch
### Q. Find the last occurrence of a target in a sorted array.

##### Code
```java
public int search(int[] arr, int k){
	int s =0;
	int e = arr.length - 1;
	
	while (s <= e){
		int mid = s + (e-s) /2;
		if (arr[mid] == k){
			if (mid == n-1 || arr[mid] != arr[mid+1]){
				return mid;
			}
			else{
				s = mid + 1;
			}
		}
		else if (arr[mid] > k){
			e = mid - 1;
		}
		else{
			s = mid + 1;
		}
	}
	
	return -1;
}
```

>TC: O(Log n)
>SC: O(1)

### #BinarySearch 
### Find the frequency of just 1 element in the array, if the array is sorted

> We know how to find the first occurrence and same we can find the last occurrence
> so, 
> 		frequency = last - first  + 1


###  #LocalMinima
## Q. Given an array of N distinct elements. Find any local minima in the array.
> Local minima => A no. which is smaller than it's adjacent neighbors.
> 
> 	if arr[1] > arr[0] 
> 		arr[0] will be local minima , and 
> 		
> 	if arr[n-1] < arr[n-2]
> 		arr[n-1] wiil be local minima 

#### Brute Force | Linear Search
>Iterate and check every element if it is a local minima or not.

> TC: O(n)

### #BinarySearch 
####  Approach 2 | Binary Search

##### Possible Cases

>Case 1: Mid is a local minima
>	if
>		mid-1 > mid && mid + 1 > mid


>Case 2: 
>	if
>		mid < mid -1        # minima is present in the right
>	
>Case 3: 
>	if
>		mid - 1 < mid && mid + 1 > mid        #  minima is present in the left
>		
>Case 4:
>	if 
>		mid - 1 < mid && mid + 1 < mid         # minima is present on both of the sides



##### Code
```java
public int findMinima(int[], int n){
	int s = 0;
	int e = n-1;
	
	while (s <= e){
		int mid = s + (e-s) / 2;
		
		if (
				(mid == 0 || arr[mid] < arr[mid-1]) &&
							(mid == n-1 || arr[mid] < arr[mid+1])
			)
		{
			return mid;					
		}
		
		else if (mid == 0 || arr[mid] < arr[mid-1]){ // go to the right
			s = mid + 1;
		}
		else{ // go to the left
			e = mid - 1;
		}
	}
	
	return -1
}
```

>TC:O(Log n)
>SC: O(1)

### #BinarySearch 
## Q. Given N, Calculate the sqrt(N).

#### Brute Force
> For each value of i do the square and compare


##### Code
```java
int ans = 1;
for (int i=2; i<N; i++){
	if (i * i <= N){
		ans = i;
	}
	else{
		break;
	}
	
	return ans;
}
```

>TC: O(sqrt(n))