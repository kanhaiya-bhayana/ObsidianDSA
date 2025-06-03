## Q1. Evaluate Expression

**Problem Description**  

An arithmetic expression is given by a string array **A** of size **N**. Evaluate the value of an arithmetic expression in **Reverse Polish Notation**.

Valid operators are +, -, *, /. Each string may be an **integer** or an **operator**.  
  
**Note**: Reverse Polish Notation is equivalent to **Postfix Expression**, where operators are written **after** their operands.  
  
**Problem Constraints**  

1 <= N <= 105
  
**Input Format**  

The only argument given is string array A.

**Output Format**  

Return the value of arithmetic expression formed using reverse Polish Notation.  
  
**Example Input**  

Input 1:

A =   ["2", "1", "+", "3", "*"]

Input 2:

A = ["4", "13", "5", "/", "+"]
  
**Example Output**  

Output 1:

9

Output 2:

6

**Example Explanation**  

Explaination 1:

starting from backside:
    * : () * ()
    3 : () * (3)
    + : (() + ()) * (3)
    1 : (() + (1)) * (3)
    2 : ((2) + (1)) * (3)
    ((2) + (1)) * (3) = 9

Explaination 2:

starting from backside:
    + : () + ()
    / : () + (() / ())
    5 : () + (() / (5))
    13 : () + ((13) / (5))
    4 : (4) + ((13) / (5))
    (4) + ((13) / (5)) = 6

Expected Output

Provide sample input and click run to see the correct output for the provided input. Use this to improve your problem understanding and test edge cases

##### Code
```java
public class Solution {

    public int evalRPN(ArrayList<String> A) {
        Stack<Integer> st = new Stack<>();
        for (String s : A){
            switch (s){
                case "+":
                    st.push(st.pop() + st.pop());
                    break;
                case "-":
                    int b = st.pop();
                    int a = st.pop();
                    st.push(a-b);
                    break;
                case "*":
                    st.push(st.pop() * st.pop());
                    break;
                case "/":
                    int divisor = st.pop();
                    int dividend = st.pop();
                    st.push(dividend / divisor);
                    break;
                default: // It's a number
                    st.push(Integer.parseInt(s));
                    break;
            }
        }
        return st.pop();
    }
}
```


## Q2. Balanced Parenthesis

**Problem Description**  

Given an expression string **A**, examine whether the pairs and the orders of `“{“,”}”, ”(“,”)”, ”[“,”]”` are correct in **A**.

Refer to the examples for more clarity.
  
**Problem Constraints**  

1 <= |A| <= 100
  
**Input Format**  

The first and the only argument of input contains the string A having the parenthesis sequence.
  
**Output Format**  

Return 0 if the parenthesis sequence is not balanced.

Return 1 if the parenthesis sequence is balanced.

**Example Input**  

Input 1:

 A = {([])}

Input 2:

 A = (){

Input 3:

 A = ()[] 

**Example Output**  

Output 1:

 1 

Output 2:

 0 

Output 3:

 1 
  
**Example Explanation**  

You can clearly see that the first and third case contain valid paranthesis.

In the second case, there is no closing bracket for {, thus the paranthesis sequence is invalid.


Expected Output

Provide sample input and click run to see the correct output for the provided input. Use this to improve your problem understanding and test edge cases


##### Code
```java
public class Solution {

    public int solve(String s) {
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
                    return 0;
                }
                char top = stack.pop();
                if ((c == ')' && top != '(') ||
                    (c == '}' && top != '{') ||
                    (c == ']' && top != '[')) {
                    return 0;
                }
            }
        }
        // If stack is empty, all brackets matched correctly
        return stack.isEmpty() ? 1 : 0;
    }
}
```


## Q3. Passing game

**Problem Description**  

There is a football event going on in your city. In this event, you are given **A** passes and players having ids between **1** and **106**.

Initially, some player with a given id had the ball in his possession. You have to make a program to display the id of the player who possessed the ball after exactly A passes.

There are two kinds of passes:

1) ID

2) 0

For the first kind of pass, the player in possession of the ball passes the ball "forward" to the player with id = ID.

For the second kind of pass, the player in possession of the ball passes the ball back to the player who had forwarded the ball to him.

In the second kind of pass "0" just means Back Pass.

Return the ID of the player who currently possesses the ball.

  
  
**Problem Constraints**  

1 <= A <= 100000

1 <= B <= 100000

|C| = A
  
**Input Format**  

The first argument of the input contains the number A.

The second argument of the input contains the number B ( id of the player possessing the ball in the very beginning).

The third argument is an array C of size A having (ID/0).
  
**Output Format**  

Return the "ID" of the player who possesses the ball after A passes.
  
**Example Input**  

Input 1:

 A = 10
 B = 23
 C = [86, 63, 60, 0, 47, 0, 99, 9, 0, 0]

Input 2:

 A = 1
 B = 1
 C = [2]
  
**Example Output**  

Output 1:

 63

Output 2:

 2
  
**Example Explanation**  

Explanation 1:

 Initially, Player having  id = 23  posses ball. 
 After pass  1,  Player having  id = 86  posses ball. 
 After pass  2,  Player having  id = 63  posses ball. 
 After pass  3,  Player having  id = 60  posses ball. 
 After pass  4,  Player having  id = 63  posses ball. 
 After pass  5,  Player having  id = 47  posses ball. 
 After pass  6,  Player having  id = 63  posses ball. 
 After pass  7,  Player having  id = 99  posses ball. 
 After pass  8,  Player having  id = 9   posses ball. 
 After pass  9,  Player having  id = 99  posses ball. 
 After pass  10, Player having  id = 63   posses ball.

Explanation 2:

 Ball is passed to 2.

Expected Output

Provide sample input and click run to see the correct output for the provided input. Use this to improve your problem understanding and test edge cases

##### Code
```java
public class Solution {

    public int solve(int A, int B, ArrayList<Integer> C) {

        Stack<Integer> st = new Stack<>();
        int curr = B;
        for (int i : C){
            if (i == 0){
                curr = st.pop();
            }
            else{
                st.push(curr);
                curr = i;
            }
        }
        return curr;
    }
}
```