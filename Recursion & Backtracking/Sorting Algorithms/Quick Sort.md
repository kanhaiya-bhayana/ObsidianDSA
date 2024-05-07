
### Divide & Conquer algorithm

1. Find the pivot and place it on their correct place
	1. Pivot can be your first element
		1. last element
		2. random element
		3. mid element

#### Time Complexity
	N Log N
#### Space Complexity
		O(1)

```java
qs(arr,low,high)
{
	if (low < high)
	{
		partitionIndex = f(arr,low,high);
		qs(arr,low,partitionIndex-1);
		qs(arr,partitionIndex+1,high);
	}
}
```

```java
int f(arr,low,high)
{
	pivot = arr[low];
	i = low;
	j = high;
	
	while (i < j)
	{
		// find the first element which is greater than the pivot
		while (arr[i] <= pivot && i <= high-1)
		{
			i++;
		}
		// find the first smallest element which is lesser than the pivot
		while (arr[j] > pivot && j>= low+1)
		{
			j--;
		}
		
		// if i or j haven't crossed then swap them
		if (i < j)
		{
			swap(arr[i],arr[j]);
		}
	}
	// insert the pivot at its correct place
	swap(arr[low],arr[j]);
	return j;
}
```