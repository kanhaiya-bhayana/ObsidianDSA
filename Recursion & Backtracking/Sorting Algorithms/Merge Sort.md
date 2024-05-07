

Divide *&* Merge

Time complexity N x log b2 N
Space complexity O(N)

```java
void mergerSort(arr, low, high)
{
	// base case
	if (low >= high)
	{
		return;
	}
	mid = (low + high) / 2;
	mergeSort(arr,low,mid);
	mergeSort(arr,mid+1,high);
	merge(arr,low,mid,high);
}
```

```java
void merge(arr, low, mid, high)
{
	int left = low;
	int right = mid + 1;
	while (left <= mid && right <= high)
	{
		if (arr[left] <= arr[right])
		{
			temp.add(arr[left]);
			left++;
		}
		else
		{
			temp.add[right];
			right++;
		}
	}
	while (left <= mid)
	{
		temp.add(arr[left]);
		left++;
	}
	while (right <= high)
	{
		temp.add(arr[right])	;
		right++;
	}
	for(int i = low; i <= high; i++)
	{
		arr[i] = temp[i - low];
	}
}
```


https://www.geeksforgeeks.org/problems/merge-sort/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article