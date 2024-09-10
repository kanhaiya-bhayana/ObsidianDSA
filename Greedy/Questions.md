## 1. [Bag of Tokens](https://leetcode.com/problems/bag-of-tokens/)

###### Explanation:
- **Goal**: The problem is to maximize the score. We can either:
  - Use power to gain a point (by spending the smallest available token).
  - Sacrifice a point to regain power (by gaining the power of the largest available token).
- **Approach**:
  - Sort the tokens to easily access the smallest and largest tokens.
  - Use a two-pointer technique (`i` for the smallest, `j` for the largest).
  - Try to gain points by spending power, and if you can't, trade points to regain power.
  - Track the maximum score throughout this process.

```java
class Solution {
    public int bagOfTokensScore(int[] tokens, int power) {
        // Call the helper method solve to calculate the maximum possible score
        return solve(tokens, power);
    }

    private int solve(int[] tokens, int power) {
        int n = tokens.length;  // Get the number of tokens
        Arrays.sort(tokens);    // Sort the tokens array in ascending order to access smallest/largest tokens efficiently
        int i = 0;              // Pointer for the smallest token (start of the array)
        int j = n - 1;          // Pointer for the largest token (end of the array)
        int score = 0;          // Initialize current score
        int maxScore = 0;       // Initialize the maximum score obtained

        // Loop while the pointers i and j haven't crossed each other
        while (i <= j) {
            // If we have enough power to play the smallest token
            if (power >= tokens[i]) {
                power -= tokens[i];  // Deduct the token's power from available power
                i++;                 // Move to the next smallest token
                score += 1;          // Gain one point of score

                // Update the maximum score if the current score exceeds the previous max
                maxScore = Math.max(maxScore, score);
            } 
            // If we don't have enough power but have at least one point of score
            else if (score >= 1) {
                power += tokens[j];  // Regain power by sacrificing the largest token
                j--;                 // Move to the next largest token
                score -= 1;          // Lose one point of score
            } 
            // If neither conditions are met, break the loop
            else {
                break;               // Exit the loop when no further moves are possible
            }
        }
        // Return the maximum score obtained
        return maxScore;
    }
}
```


## 2. [Boats to Save People](https://leetcode.com/problems/boats-to-save-people/)
###### Explanation:
- **Goal**: The task is to calculate the minimum number of rescue boats required to save everyone, given that each boat has a weight limit.
- **Approach**:
  - Sort the array `people` so that we can easily pair the lightest and heaviest person.
  - Use a two-pointer technique (`i` for the lightest, `j` for the heaviest).
  - If the sum of the lightest and heaviest person's weights exceeds the boat's weight limit, send the heaviest person alone.
  - If they can share a boat, send both of them.
  - Each iteration uses a boat, so increment the `boats` counter.
- **Result**: The algorithm returns the minimum number of boats required to rescue all people.

```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        // Call the helper method solve to calculate the minimum number of boats required
        return solve(people, limit);
    }

    private int solve(int[] people, int limit) {
        int i = 0; // Pointer for the lightest person (start of the array)
        int j = people.length - 1; // Pointer for the heaviest person (end of the array)
        int boats = 0; // Initialize the number of boats needed
        
        Arrays.sort(people); // Sort the array to facilitate pairing people by weight

        // Loop while the two pointers haven't crossed each other
        while (i <= j) {
            // If the sum of the lightest and heaviest person exceeds the boat's weight limit
            if (people[i] + people[j] > limit) {
                // The heaviest person goes alone in a boat, decrement the j pointer
                j--;
            } 
            // If the lightest and heaviest person can share the same boat
            else {
                // Both can go in the same boat, so move both pointers
                i++;
                j--;
            }
            // Increment the boat count for each boat used
            boats += 1;
        }
        // Return the total number of boats needed
        return boats;
    }
}
```


## 3. [Break a Palindrome](https://leetcode.com/problems/break-a-palindrome/)
###### Explanation:
- **Goal**: The task is to break the given palindrome by changing exactly one character such that the resulting string is not a palindrome anymore and lexicographically smallest.
- **Approach**:
  - First, check if the length of the palindrome is 1. If so, return an empty string because it's impossible to break a single-character palindrome.
  - Convert the string into a mutable `StringBuilder` to allow character modification.
  - Iterate through the first half of the palindrome:
    - If any character is not 'a', replace it with 'a' and return the string immediately, as this would be the lexicographically smallest change.
  - If the entire first half consists of 'a's, change the last character to 'b' to break the palindrome, as no smaller lexicographical changes are possible.
- **Result**: The method returns the modified string, which is no longer a palindrome and is lexicographically smallest.


```java
class Solution {
    public String breakPalindrome(String palindrome) {
        // Call the helper method solve to break the palindrome
        return solve(palindrome);
    }

    private String solve(String palindrome) {
        int n = palindrome.length(); // Get the length of the palindrome

        // If the length of the palindrome is 1, it's impossible to break it, return an empty string
        if (n == 1) {
            return "";
        }

        // Convert the palindrome string into a mutable StringBuilder object
        StringBuilder str = new StringBuilder(palindrome);

        // Loop through the first half of the string
        for (int i = 0; i < n / 2; i++) {
            // If the current character is not 'a', we can replace it with 'a'
            if (str.charAt(i) != 'a') {
                str.setCharAt(i, 'a'); // Replace the character with 'a'
                return str.toString(); // Return the modified string, breaking the palindrome
            }
        }

        // If the entire first half is made of 'a's, replace the last character with 'b'
        str.setCharAt(n - 1, 'b'); 
        return str.toString(); // Return the modified string
    }
}
```





