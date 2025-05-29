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
