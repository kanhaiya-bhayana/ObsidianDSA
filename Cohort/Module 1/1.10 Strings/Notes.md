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