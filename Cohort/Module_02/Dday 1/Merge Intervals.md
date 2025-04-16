Define by start and end time
-> start <= end


#### Given some intervals, merge them if they intersect

#### Approach #Approach1 
```
1. Create an array to store the merged intervals
2. Start with 0th interval in your hand
3. From i = 1 to n - 1
	1. if ith interval is intersect:
			merge it with inhand interval and updat inhand interval
	2. else
			Add interval hand to the output, and take ith interva in hand
4.

Make sure given interval array is sorted

currS = arr[0][0];
currE = arr[0][1];

for (i = 1 to N - 1){
	if (arr[i][0] <= currE){
		currE = Math.max(currE, arr[i][1]);
	}
	else{
		ans.add({currS, currE});
		currS = arr[i][0];
		currE = arr[i][1];
	}
}
currE = Math.max(currE, arr[i][1]);
```


```
TC: O(N)
SC: O(1)
```

