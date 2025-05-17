
## Agenda
### 1. Modular Arithmetic
### 2. Mod on power function
### 3. Pairs whose mod sum is 0
### 4. Introduction to GCD
### 5. Properties of GCD

---


> A % B = Dividend - Highest multiple of divisor <= Dividend

```
1. ( a + b ) % M = ( (a % M) + (b % M) ) % M
2. ( a * b ) % M = ( (a % M) * (b % M) ) % M
3. (a + M) % M = a % M
4. (a - M) % M = ( (a % M) - (b % M) + M ) % M
5. (a ^ b) % M = ( (a % M)^b ) % M
```


### Q. Given an integer array of size N and an int M. Find the count of pairs (i,j) such that (A[i] + A[j]) % M == 0

#### Brute Force

##### Code
```java
public int func(int[] arr, int M){
	int n = arr.length;
	int cnt = 0;
	
	for (int i=0; i<n; i++){
		for (int j = (i+1); j<n; j++){
			if ((arr[i] + arr[j]) % M == 0){
				cnt += 1;
			}
		}
	}
	
	return cnt;
}
```

> TC = O (n^2)
> SC = O (1)

