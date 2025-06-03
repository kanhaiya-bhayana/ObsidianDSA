## Prefix Infix Postfix Conversions

##### Infix to Postfix
##### Infix to Prefix
##### Postfix to infix
##### Prefix to infix
##### Prefix to Postfix
##### Postfix to Prefix
---
## Infix to Postfix

```java
import java.util.*;

public class InfixToPostfix {

    // Function to return precedence of operators
    static int precedence(char ch) {
        switch (ch) {
            case '+':
            case '-':
                return 1;
            case '*':
            case '/':
                return 2;
            case '^':
                return 3;
        }
        return -1;
    }

    // Function to convert infix to postfix
    static String infixToPostfix(String expression) {
        StringBuilder result = new StringBuilder();
        Stack<Character> stack = new Stack<>();

        for (int i = 0; i < expression.length(); ++i) {
            char c = expression.charAt(i);

            // Skip whitespace
            if (Character.isWhitespace(c))
                continue;

            // If operand, add to output
            if (Character.isLetterOrDigit(c)) {
                result.append(c);
            }
            // If '(', push to stack
            else if (c == '(') {
                stack.push(c);
            }
            // If ')', pop and output from the stack
            // until an '(' is encountered
            else if (c == ')') {
                while (!stack.isEmpty() && stack.peek() != '(')
                    result.append(stack.pop());
                if (!stack.isEmpty() && stack.peek() != '(')
                    return "Invalid Expression"; // Invalid expression
                else
                    stack.pop();
            }
            // An operator is encountered
            else {
                while (!stack.isEmpty() && precedence(c) <= precedence(stack.peek())) {
                    result.append(stack.pop());
                }
                stack.push(c);
            }
        }

        // Pop all the operators from the stack
        while (!stack.isEmpty()) {
            if (stack.peek() == '(')
                return "Invalid Expression";
            result.append(stack.pop());
        }

        return result.toString();
    }

    public static void main(String[] args) {
        String expression = "a+b*(c^d-e)^(f+g*h)-i";
        String postfix = infixToPostfix(expression);
        System.out.println("Postfix: " + postfix);
    }
}
```

The **time** and **space** complexity of the infix-to-postfix conversion algorithm provided is as follows:
#### ✅ **Time Complexity: `O(n)`**

Where `n` is the length of the infix expression.
#### Why:

- Each character in the expression is processed exactly once.
    
- Stack operations (`push`, `pop`, `peek`) all take constant time: `O(1)`.
    
- Even when popping from the stack due to operator precedence, each operator is pushed and popped at most once.
    
> So the total work done is linear in terms of the number of characters: **`O(n)`**.

---
#### ✅ **Space Complexity: `O(n)`**

#### Breakdown:

- **Stack**: In the worst case (e.g., all operators are stacked without popping), the stack can grow up to size `n`.
    
- **Output (Postfix) String**: Stores all `n` characters from the infix expression (without parentheses), so also `O(n)`.

> Therefore, both the stack and the result string require **`O(n)`** space in the worst case.

---
#### Summary

|Resource|Complexity|
|---|---|
|Time|O(n)|
|Space (Auxiliary)|O(n)|

---

## Infix to Prefix

---

#### ✅ **Steps to Convert Infix to Prefix**

Given an infix expression, for example:  
`(A-B/C)*(A/K-L)`

---

#### 🔁 **Step 1: Reverse the Infix Expression**

- Reverse the string.
    
- Replace `'('` with `')'` and `')'` with `'('`.
    

Example:  
Original: `(A-B/C)*(A/K-L)`  
Reversed and swapped: `(L-K/A)*(C/B-A)`

---

#### 🔁 **Step 2: Convert the Modified Infix to Postfix**

Use the **same infix-to-postfix** algorithm.

Postfix of `(L-K/A)*(C/B-A)` is:  
`LKA/-CB/A-*`

---

#### 🔁 **Step 3: Reverse the Postfix Expression**

Reverse the postfix result to get the final **prefix expression**.

Reversed: `*-A/CB-/AKL`

---

#### ✅ **Java Code: Infix to Prefix Conversion**

```java
import java.util.*;

public class InfixToPrefix {

    static int precedence(char ch) {
        switch (ch) {
            case '+':
            case '-': return 1;
            case '*':
            case '/': return 2;
            case '^': return 3;
        }
        return -1;
    }

    static String infixToPostfix(String expression) {
        StringBuilder result = new StringBuilder();
        Stack<Character> stack = new Stack<>();

        for (int i = 0; i < expression.length(); i++) {
            char c = expression.charAt(i);

            if (Character.isLetterOrDigit(c)) {
                result.append(c);
            } else if (c == '(') {
                stack.push(c);
            } else if (c == ')') {
                while (!stack.isEmpty() && stack.peek() != '(') {
                    result.append(stack.pop());
                }
                if (!stack.isEmpty()) {
                    stack.pop();
                }
            } else { // Operator
                while (!stack.isEmpty() && precedence(c) <= precedence(stack.peek())) {
                    result.append(stack.pop());
                }
                stack.push(c);
            }
        }

        while (!stack.isEmpty()) {
            result.append(stack.pop());
        }

        return result.toString();
    }

    static String reverseAndSwapParentheses(String expr) {
        StringBuilder reversed = new StringBuilder();
        for (int i = expr.length() - 1; i >= 0; i--) {
            char c = expr.charAt(i);
            if (c == '(') {
                reversed.append(')');
            } else if (c == ')') {
                reversed.append('(');
            } else {
                reversed.append(c);
            }
        }
        return reversed.toString();
    }

    static String infixToPrefix(String infix) {
        String reversed = reverseAndSwapParentheses(infix);
        String postfix = infixToPostfix(reversed);
        return new StringBuilder(postfix).reverse().toString();
    }

    public static void main(String[] args) {
        String infix = "(A-B/C)*(A/K-L)";
        String prefix = infixToPrefix(infix);
        System.out.println("Prefix: " + prefix);
    }
}
```

---

#### ✅ **Output**

```
Prefix: *-A/BC-/AKL
```

#### ✅ **Time Complexity of Infix to Prefix Conversion**

The process has **three steps**:

---

#### 🔁 Step 1: Reverse and Swap Parentheses

- Reverse the string and swap `'(' ↔ ')'` in one pass.
    
- **Time Complexity**: `O(n)`
    

---

#### 🔁 Step 2: Infix to Postfix Conversion

- Each character is processed once.
    
- Stack operations (`push`, `pop`, `peek`) are `O(1)`.
    
- **Time Complexity**: `O(n)`
    

---

#### 🔁 Step 3: Reverse the Postfix String

- Reverse a string of length `n`.
    
- **Time Complexity**: `O(n)`
    

---

#### ✅ **Total Time Complexity: `O(n)`**

Where `n` is the length of the infix expression.

---

#### ✅ **Space Complexity: `O(n)`**

- **Stack**: Stores operators → up to `O(n)`
    
- **Reversed expression**, **Postfix string**, and **final Prefix** → all are `O(n)`
    

---

#### 🔚 Final Complexity Summary

| Operation       | Time   | Space  |
| --------------- | ------ | ------ |
| Reverse + Swap  | `O(n)` | `O(n)` |
| Infix → Postfix | `O(n)` | `O(n)` |
| Reverse Postfix | `O(n)` | `O(n)` |
| **Total**       | `O(n)` | `O(n)` |


## Postfix to Infix

#### ✅ Postfix to Infix Conversion: Algorithm

**Basic Idea:**

- Scan the postfix expression **left to right**.
    
- If the symbol is an **operand**, push it to the stack.
    
- If the symbol is an **operator**, pop the top two operands from the stack, combine them into an infix string with the operator in between, and **push the new string back**.
    

---
#### ✅ Example

Postfix: `ab+c*`  
Step-by-step:

1. `a` → push
    
2. `b` → push
    
3. `+` → pop `a`, `b` → form `(a+b)` → push
    
4. `c` → push
    
5. `*` → pop `(a+b)`, `c` → form `((a+b)*c)` → push
    

Final result: `((a+b)*c)`

---

#### ✅ Java Code: Postfix to Infix

```java
import java.util.*;

public class PostfixToInfix {

    static boolean isOperator(char c) {
        return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
    }

    static String postfixToInfix(String postfix) {
        Stack<String> stack = new Stack<>();

        for (int i = 0; i < postfix.length(); i++) {
            char c = postfix.charAt(i);

            // Skip whitespace
            if (Character.isWhitespace(c))
                continue;

            if (Character.isLetterOrDigit(c)) {
                stack.push(String.valueOf(c));
            } else if (isOperator(c)) {
                String op2 = stack.pop();
                String op1 = stack.pop();
                String expr = "(" + op1 + c + op2 + ")";
                stack.push(expr);
            } else {
                throw new IllegalArgumentException("Invalid character: " + c);
            }
        }

        return stack.peek();
    }

    public static void main(String[] args) {
        String postfix = "ab+c*";
        String infix = postfixToInfix(postfix);
        System.out.println("Infix: " + infix); // Output: ((a+b)*c)
    }
}
```

--
#### ✅ Time and Space Complexity

|Metric|Complexity|
|---|---|
|Time|`O(n)`|
|Space (Stack)|`O(n)`|

- Each character is processed once → `O(n)`
    
- Stack can grow up to `n`/2 entries in the worst case.
    

---

## Prefix to Infix
#### ✅ Prefix to Infix: Algorithm

**Basic Idea:**

- Scan the prefix expression **from right to left**.
    
- If the symbol is an **operand**, push it to the stack.
    
- If the symbol is an **operator**, pop the top two operands from the stack, combine them into an infix string with the operator between them (and parentheses), and push it back.
    

---

#### ✅ Example

Prefix: `*+ab-cd`

Step-by-step (right to left):

```
Stack starts empty

Read d → push("d")
Read c → push("c")
Read - → pop "c", "d" → push("(c-d)")
Read b → push("b")
Read a → push("a")
Read + → pop "a", "b" → push("(a+b)")
Read * → pop "(a+b)", "(c-d)" → push("((a+b)*(c-d))")
```

Final result: `((a+b)*(c-d))`

---

#### ✅ Java Code: Prefix to Infix

```java
import java.util.*;

public class PrefixToInfix {

    static boolean isOperator(char c) {
        return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
    }

    static String prefixToInfix(String prefix) {
        Stack<String> stack = new Stack<>();

        for (int i = prefix.length() - 1; i >= 0; i--) {
            char c = prefix.charAt(i);

            if (Character.isWhitespace(c))
                continue;

            if (Character.isLetterOrDigit(c)) {
                stack.push(String.valueOf(c));
            } else if (isOperator(c)) {
                String op1 = stack.pop();
                String op2 = stack.pop();
                String expr = "(" + op1 + c + op2 + ")";
                stack.push(expr);
            } else {
                throw new IllegalArgumentException("Invalid character: " + c);
            }
        }

        return stack.peek();
    }

    public static void main(String[] args) {
        String prefix = "*+ab-cd";
        String infix = prefixToInfix(prefix);
        System.out.println("Infix: " + infix); // Output: ((a+b)*(c-d))
    }
}
```

---

#### ✅ Time and Space Complexity

|Metric|Complexity|
|---|---|
|Time|`O(n)`|
|Space (Stack)|`O(n)`|

Each symbol is processed once, and stack operations are constant time.

---

## Prefix to Postfix

#### ✅ Prefix to Postfix: Algorithm

**Basic Idea:**

- **Scan the prefix expression from right to left.**
    
- If the symbol is an **operand**, push it onto the stack.
    
- If the symbol is an **operator**, pop the top two operands, combine them into a **postfix expression**: `operand1 operand2 operator`, and push that back onto the stack.
    

---

#### ✅ Example

Prefix: `*+ab-cd`

Steps (right to left):

```
Read d → push("d")
Read c → push("c")
Read - → pop "c", "d" → push("cd-")
Read b → push("b")
Read a → push("a")
Read + → pop "a", "b" → push("ab+")
Read * → pop "ab+", "cd-" → push("ab+cd-*")
```

✅ Final result: `ab+cd-*`

---

#### ✅ Java Code: Prefix to Postfix

```java
import java.util.*;

public class PrefixToPostfix {

    static boolean isOperator(char c) {
        return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
    }

    static String prefixToPostfix(String prefix) {
        Stack<String> stack = new Stack<>();

        for (int i = prefix.length() - 1; i >= 0; i--) {
            char c = prefix.charAt(i);

            if (Character.isWhitespace(c))
                continue;

            if (Character.isLetterOrDigit(c)) {
                stack.push(String.valueOf(c));
            } else if (isOperator(c)) {
                String op1 = stack.pop();
                String op2 = stack.pop();
                String expr = op1 + op2 + c;
                stack.push(expr);
            } else {
                throw new IllegalArgumentException("Invalid character: " + c);
            }
        }

        return stack.peek();
    }

    public static void main(String[] args) {
        String prefix = "*+ab-cd";
        String postfix = prefixToPostfix(prefix);
        System.out.println("Postfix: " + postfix); // Output: ab+cd-*
    }
}
```

---

#### ✅ Time and Space Complexity

|Metric|Complexity|
|---|---|
|Time|O(n)|
|Space (Stack)|O(n)|

---
## Postfix to Prefix
#### ✅ Postfix to Prefix: Algorithm

**Idea:**

- Scan the **postfix expression left to right**.
    
- If you encounter an **operand**, push it to the stack.
    
- If you encounter an **operator**, pop the **top two operands**, combine them into a prefix expression (`operator + op1 + op2`), and push that back to the stack.
    

---

#### ✅ Example

Postfix: `ab+c*`

Steps:

```
Read a → push("a")
Read b → push("b")
Read + → pop "a", "b" → push("+ab")
Read c → push("c")
Read * → pop "+ab", "c" → push("*+abc")
```

✅ Final result: `*+abc`

---

#### ✅ Java Code: Postfix to Prefix

```java
import java.util.*;

public class PostfixToPrefix {

    static boolean isOperator(char c) {
        return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
    }

    static String postfixToPrefix(String postfix) {
        Stack<String> stack = new Stack<>();

        for (int i = 0; i < postfix.length(); i++) {
            char c = postfix.charAt(i);

            if (Character.isWhitespace(c))
                continue;

            if (Character.isLetterOrDigit(c)) {
                stack.push(String.valueOf(c));
            } else if (isOperator(c)) {
                String op2 = stack.pop();
                String op1 = stack.pop();
                String expr = c + op1 + op2;
                stack.push(expr);
            } else {
                throw new IllegalArgumentException("Invalid character: " + c);
            }
        }

        return stack.peek();
    }

    public static void main(String[] args) {
        String postfix = "ab+c*";
        String prefix = postfixToPrefix(postfix);
        System.out.println("Prefix: " + prefix); // Output: *+abc
    }
}
```

---

#### ✅ Time and Space Complexity

|Metric|Complexity|
|---|---|
|Time|O(n)|
|Space (Stack)|O(n)|

---
