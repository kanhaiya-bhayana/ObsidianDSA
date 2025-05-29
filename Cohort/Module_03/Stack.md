## Valid Parentheses 

```java
import java.util.Stack;

public class ValidParentheses {
    public static boolean isValid(String s) {
        // Create a stack to hold opening parentheses
        Stack<Character> stack = new Stack<>();
        
        // Traverse through the string
        for (char c : s.toCharArray()) {
            // Push opening brackets onto the stack
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            }
            // Handle closing brackets
            else if (c == ')' || c == '}' || c == ']') {
                // If stack is empty or top of stack doesn't match, return false
                if (stack.isEmpty()) {
                    return false;
                }
                
                char top = stack.pop();
                if ((c == ')' && top != '(') || 
                    (c == '}' && top != '{') || 
                    (c == ']' && top != '[')) {
                    return false;
                }
            }
        }
        
        // If stack is empty, all brackets matched correctly
        return stack.isEmpty();
    }

    public static void main(String[] args) {
        // Test cases
        System.out.println(isValid("()"));       // true
        System.out.println(isValid("()[]{}"));   // true
        System.out.println(isValid("(]"));       // false
        System.out.println(isValid("([)]"));     // false
        System.out.println(isValid("{[]}"));     // true
    }
}
```

>TC: O(n), where 𝑛 is the length of the input string, 
>and 
>SC: O(n) space complexity for the stack.


### Using HashMap and Stack
> This simplifies the conditionals when handling closing brackets and makes the code more maintainable.
```java
class Solution {
    public boolean isValid(String s) {
        Map<Character,Character> map = new HashMap<>();
        seedData(map);

        Stack<Character> st = new Stack<>();

        for (char c : s.toCharArray()){
            if (map.containsKey(c)){
                char top = st.isEmpty() ? '#' : st.pop();

                if (top != map.get(c)){
                    return false;
                }
            }
            else{
                st.push(c);
            }
        }

        return st.isEmpty();
    }

    private void seedData(Map<Character,Character> map){
        map.put(')', '(');
        map.put(']', '[');
        map.put('}', '{');
    }
}
```
>TC: O(n), where 𝑛 is the length of the input string, 
>and 
>SC: O(n) space complexity for the stack.

### Solution using array

```java
class Solution {
    public boolean isValid(String s) {
        if (s.length() % 2 != 0) return false;
        var stack = new char[s.length() / 2];
        var pointer = 0;
        for (var ch : s.toCharArray()) {
            if (ch == '(' || ch == '{' || ch == '[') {
                if (pointer == stack.length) return false;
                stack[pointer++] = ch;
            } else {
                if (pointer == 0) return false;
                var ch1 = stack[--pointer];
                if ((ch == ')' && ch1 != '(') || (ch == '}' && ch1 != '{') || (ch == ']' && ch1 != '[')) {
                    return false;
                }
            }
        }
        return pointer == 0;
    }
}
```


### **Time Complexity**

The **time complexity** of this code is $O(n)$, where $n$ is the length of the input string.

#### Explanation:

* The code iterates through each character of the input string once in the `for` loop.
* All operations inside the loop (including comparisons, stack push/pop emulation with the array) are $O(1)$.
* Thus, the overall time complexity is $O(n)$.

---

### **Space Complexity**

The **space complexity** of this code is $O(n/2) = O(n)$, where $n$ is the length of the input string.

#### Explanation:

* The `stack` array is allocated with a size of $n/2$ to hold only the opening brackets.
* At most, half the characters in the string can be opening brackets (e.g., `(((((`), which is why the stack size is $n/2$.
* Additional variables like `pointer` and `ch1` use constant space $O(1)$.

Since $n/2$ is proportional to $n$, the space complexity is $O(n)$.

---

### **Summary**

| Aspect               | Value  |
| -------------------- | ------ |
| **Time Complexity**  | $O(n)$ |
| **Space Complexity** | $O(n)$ |

This implementation is efficient in terms of both time and space, with space usage optimized by preallocating a fixed-size array instead of a dynamic `Stack`.

