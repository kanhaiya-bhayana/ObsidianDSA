### Bitwise Operations: Key Properties
#### **Basic AND (`&amp;`)**
```  
1. A &amp; 0 = 0  
2. A &amp; A = A  
```
#### **Basic OR (`|`)**
```  
1. A | 0 = A  
2. A | A = A  
```
#### **Basic XOR (`^`)**
```  
1. A ^ 0 = A  
2. A ^ A = 0  
```

---
### **Commutative Property**
```  
A &amp; B = B &amp; A  
A | B = B | A  
A ^ B = B ^ A  
```
### **Associative Property**
**AND**
```  
(A &amp; B) &amp; C = A &amp; (B &amp; C)  
```

**OR**
```  
(A | B) | C = A | (B | C)  
```

**XOR**
```  
(A ^ B) ^ C = A ^ (B ^ C)  
```

---
## Even/Odd Number Check Using Bit Mask

- **Mask:** Use `mask = 1` (binary `0001`).
    
- **Operation:** Perform bitwise AND: `number & mask`.
    
- **Result:**
    
    - If result is `1` → **Odd**
        
    - If result is `0` → **Even**
        

**Example:**

- For `A = 10` (binary `1010`): `1010 & 0001 = 0000` → **Even**
    
- For `B = 11` (binary `1011`): `1011 & 0001 = 0001` → **Odd**
    

---
### #BitManipulation
### Q. Given an Integer array where every element occurs twice except one which element occurs exactly one. Find the unique element.

#### Approach 
>Take the XOR of all the elements and return the result.
##### Code
```java
func(int[] arr){
	int ans = 0;
	
	for (int i : n){
		ans ^= i;
	}
	
	return ans;
}
```

> TC: O(N)
> SC: O(1)

## Shift Operators

### 1. Left Shift (<<)
### 2. Right Shift (>>)


### 1. Left Shift
> A << x => Shift all bits of A, x positions to the left.

As long as no overflow happens
	(A<<x) = Ax2^x
	(1<<i) = 2^i
	2^i = (1<<i)
### Q. Given an integer N and an integer i. Set the ith bit in N
> OR opearator
		N = N|(1<<i) 


### Q. Given an integer N and an integer i. Toggle the ith bit in N
> XOR operator
		N = N^(1<<i)


### Q. Given an integer N and an integer i. Check if the ith bit is set or not
```java
boolean isCheck(int N, int i){
	if ((N&(1<<i))){
		return false;
	}
	else{
		return true;
	}
}
```

### Q. Given an integer N, Count the number of set bits in an integer.
```java
func(int N){
	int cnt = 0;
	for (int i=0; i<32; i++){
		if (isCheck(N,i)){
			count++;
		}
	}
}

```