### Flip and Print

```java
public void flipAndPrint(String s){
	StringBuilder str = new StringBuilder("");
	for (char c : s.toCharArray()){
		if (c >= 'a' && c <= 'z'){
			str.append((char)(c-32));
		}
		else{
			str.append((char)(c+32));
		}
	}
	System.out.println(str.toString());
}


// OR

public void flipAndPrint(String s){
	char[] str = new char[s.length()];
	int i = 0;
	for (char c : s.toCharArray()){
		if (c >= 'a' && c <= 'z'){
			str[i++] = (char)(c-32);
		}
		else{
			str[i++] = (char)(c+32);
		}
	}
	System.out.println(new String(str));
}

// Time Complexity: O(n)
// Space Complexity: O(n)
```

### For a string of length **N**, the total number of substrings can be calculated as follows:

![[s1.png]]
![[s2.png]]

### Check a substring is palindrome or not

```java
public void isPalindrome(String s){
	int n = s.length();
	int s = 0;
	int e = n-1;

	while (s < e){
		if (s.charAt(s) != s.charAt(e)){
			return false;
		}
		else{
			s++;
			e--;
		}
	}
	
	return true;
}

// Time Complexity: O(n)
// Space Complexity: O(1)
```


### Longest Palindromic Substring

##### Brute Force

```
Time Complexity -> O(n^3)
Time Complexity -> O(1)
```

##### Optimized 

###### Observations

1. Odd length -> (find possible center, and move start and end to the opposite directions)
2. Even length -> (two adjacent elements should be same, and then start moving s and e into opposite directions)


```java
public int longestPalindromicSubstring(char[] s){
	int max = 0;
	int n = s.length;
	
	for (int i=0; i<n; i++){
		// odd length
		// if this is an odd length substring then, s and e are pointing to the same index
		int left, right = i;
		while (left >= 0 && right < n){
			if (s[left] != s[right]){
				break;
			}
			left--;
			right++;
		}
		max = Math.max(max, right-left-1);
		
		// even lenght substring
		left = i;
		right = i+1;
		while (left >= 0 && right < n){
			if (s[left] != s[right]){
				break;
			}
			left--;
			right++;
		}
		max = Math.max(max, right-left-1);
	}
	
	return max;
}


// or 

public int longestPalindromicSubstring(String s){
	int max = 0;
	int n = s.length;
	int left = 0;
	
	for (int i=0; i<n; i++){
		
		int len1 = expandAroundCenter(s, i, i); // odd length
		int len2 = expandAroundCenter(s, i, i+1); // even length
		int len = Math.max(len1, len2);
		
		if (len > max){
			max = len;
			start = i - (len - 1)/2;
		}
	}
	
	return s.substring(start, start + max);
}

private int expandAroundCenter(String s, int left, int right){
	while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)){
		left++;
		right--;
	}
	return right-left-1;
}

// Time Complexity -> O(n^2)
// Time Complexity -> O(1)

```