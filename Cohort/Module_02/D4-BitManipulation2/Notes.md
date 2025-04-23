### If count the setbits using the right shift operator then 
```java
public int func(int n){
	int cnt = 0;
	
	while (n > 0){
		if ((n & 1) == 1)
			cnt++;
		n = n >> 1;
	}
	
	return cnt;
}
```

```
TC: O(log n)
SC: O(1)
```


### Given an integer array where every element occurs thrice instead of one element that occurs exactly once. Find that element?

#### Brute force
> For every element count the no. of occurrences


```
- Iterate through each element of the array.
- For each element, count the number of times it appears in the array.
- If the count is not 3, return that element.
```

```
TC: O(n^2)
SC: O(1)
```