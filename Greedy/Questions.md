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



## 4. [Broken Calculator](https://leetcode.com/problems/broken-calculator/)
###### Explanation:
1. **Base Case**: If `startValue` (denoted as `s`) is greater than or equal to `target` (denoted as `t`), we simply subtract the target from the start value because the only valid operation in this case is decrementing `startValue`.
  
2. **Recursive Case (Target is Even)**: If the target is even, the best way to reduce the target is by dividing it by 2, which helps reduce the problem size faster than subtracting 1. The recursion continues from there.

3. **Recursive Case (Target is Odd)**: If the target is odd, we first increment it by 1 to make it even. This allows us to apply the division-by-2 step in the next recursion.

```java
class Solution {
    public int brokenCalc(int startValue, int target) {
        // This method simply calls the solve method to calculate the minimum steps.
        return solve(startValue, target);
    }

    private int solve(int s, int t) {
        // Base case: If the start value is greater than or equal to the target,
        // the only way to reach the target is by decrementing (subtracting 1).
        if (s >= t) {
            return s - t; // Subtract directly since we can only decrement.
        }

        // If the target is even, the optimal move is to divide it by 2.
        // This reduces the target more quickly than subtracting 1.
        if (t % 2 == 0) {
            return 1 + solve(s, t / 2); // Increment the count by 1 (for this division step) and recurse.
        }

        // If the target is odd, the optimal move is to increment the target by 1
        // to make it even, and then divide by 2 in the next recursive call.
        return 1 + solve(s, t + 1); // Increment the count by 1 (for this increment step) and recurse.
    }
}
```


## 5. [Minimum Time to Make Rope Colorful](https://leetcode.com/problems/minimum-time-to-make-rope-colorful/)

### Intuition:
This problem aims to minimize the cost required to remove consecutive balloons of the same color. For each consecutive group of balloons with the same color, you want to keep the balloon that requires the most time to remove and delete the rest. The total cost to remove the balloons will be the sum of the minimum times needed to remove balloons from each group.
### Step-by-Step Explanation:
1. **Initial Setup**: 
   - We declare two variables: `time` (to accumulate the total removal cost) and `prevMax` (to track the largest removal time for a group of consecutive balloons).
   
2. **Loop Through Balloons**:
   - For each balloon, we check if it's the same color as the previous one.
   - If it is a different color, we reset `prevMax` to 0 because we are starting a new group.

3. **Cost Calculation**:
   - For each balloon in a consecutive group, we add the minimum of `prevMax` and the current balloon's removal cost to `time`.
   - This represents removing the cheaper balloons and keeping the most expensive one.
   
4. **Update Maximum Time**:
   - After deciding which balloon to remove, update `prevMax` to the maximum value between the current and previous balloon's removal costs.

5. **Return Result**:
   - Once the loop finishes, we return the total `time` as the result. This is the minimum cost needed to remove the consecutive same-colored balloons.
### Code Explanation:

```java
class Solution {
    public int minCost(String colors, int[] neededTime) {
        return solve(colors, neededTime);  // Call the helper method 'solve' to calculate the minimum time.
    }

    private int solve(String colors, int[] neededTime) {
        int time = 0;      // 'time' will store the total cost of removing balloons.
        int prevMax = 0;   // 'prevMax' stores the maximum time needed to remove the previous balloon in a consecutive sequence.
        int n = colors.length();  // 'n' is the total number of balloons.

        // Loop through all the balloons
        for (int i = 0; i < n; i++) {
            // If the current balloon's color is different from the previous one, reset 'prevMax'.
            if (i > 0 && colors.charAt(i) != colors.charAt(i - 1)) {
                prevMax = 0;  // No need to compare with previous balloons since they have different colors.
            }

            int curr = neededTime[i];  // Current time required to remove the current balloon.

            // Add the smaller time (either the previous max or current time) to the total time.
            // This ensures that we only add the lower time to remove one of the duplicate colored balloons.
            time += Math.min(prevMax, curr);

            // Update 'prevMax' to store the larger time between the current and previous max.
            // This way, we ensure that we are keeping the balloon with the maximum removal cost.
            prevMax = Math.max(curr, prevMax);
        }

        return time;  // Return the total cost of removing the balloons.
    }
}
```

## 6. [Earliest Possible Day of Full Bloom](https://leetcode.com/problems/earliest-possible-day-of-full-bloom/)
### Intuition:
To minimize the overall time required for all plants to reach full bloom, we should prioritize planting flowers that have longer grow times. By doing so, the plants with the longest grow times start growing earlier, reducing the overall time to bloom all plants. The approach involves sorting the plants based on their `growTime` in descending order and then calculating the bloom time for each plant.

### Step-by-Step Explanation:
1. **Initial Setup**:
   - We create a list of pairs that store each plant's `plantTime` and `growTime`.
   - The idea is to first sort the plants based on their `growTime` in descending order, so that the plants which take the longest to grow are planted as early as possible.

2. **Sorting**:
   - Using `Collections.sort`, we sort the list by `growTime` in descending order.
   - By planting flowers with longer `growTime` first, we reduce the waiting time for the final bloom.

3. **Tracking Planting and Bloom Time**:
   - We maintain two variables:
     - `prevPlantDays` to accumulate the total time spent planting flowers.
     - `maxBloomDays` to track the earliest full bloom time of all flowers.
   
4. **Loop Through Plants**:
   - For each plant in the sorted list, we update the `prevPlantDays` by adding the current plant’s `plantTime`.
   - We then calculate the bloom time of the current plant by adding its `growTime` to `prevPlantDays`.

5. **Updating Maximum Bloom Days**:
   - We update `maxBloomDays` with the maximum bloom time encountered so far.
   - This ensures that we capture the latest bloom time, which represents the earliest day on which all plants are fully bloomed.

6. **Return Result**:
   - After processing all plants, `maxBloomDays` will contain the earliest full bloom time for all plants.

This approach ensures that the flowers with the longest grow time are given priority, reducing the overall time to achieve full bloom for all flowers.
### Code Explanation:

```java
class Solution {
    public int earliestFullBloom(int[] plantTime, int[] growTime) {
        return solve(plantTime, growTime);  // Call the helper method 'solve' to calculate the earliest full bloom time.
    }

    private int solve(int[] plantTime, int[] growTime) {
        List<Pair> list = new ArrayList<>();  // List to store each plant's plantTime and growTime as a Pair.
        int n = plantTime.length;

        // Create a list of Pairs (plantTime, growTime) for each plant.
        for (int i = 0; i < n; i++) {
            list.add(new Pair(plantTime[i], growTime[i]));
        }

        // Sort the list by growTime in descending order, so plants with longer grow times are planted first.
        Collections.sort(list, (a, b) -> b.growTime - a.growTime);

        int maxBloomDays = 0;  // This will store the maximum bloom time (earliest full bloom day).
        int prevPlantDays = 0; // This will accumulate the total planting time as we plant each flower.

        // Loop through the sorted list of pairs (plantTime, growTime)
        for (int i = 0; i < n; i++) {
            Pair curr = list.get(i);
            int currPlantTime = curr.plantTime;  // Time required to plant the current flower.
            int currGrowTime = curr.growTime;    // Time required for the current flower to fully grow.

            // Add the current plant's planting time to the total planting days.
            prevPlantDays += currPlantTime;

            // Calculate when the current plant will fully bloom.
            int currPlantBloomTime = prevPlantDays + currGrowTime;

            // Update the maximum bloom days if the current bloom time exceeds the previous maximum.
            maxBloomDays = Math.max(currPlantBloomTime, maxBloomDays);
        }

        return maxBloomDays;  // Return the earliest possible full bloom time.
    }

    // Helper class to store plantTime and growTime as a pair.
    class Pair {
        int plantTime;
        int growTime;
        Pair(int p, int g) {
            plantTime = p;
            growTime = g;
        }
    }
}
```


## 7. [Longest Palindrome by Concatenating Two Letter Words](https://leetcode.com/problems/longest-palindrome-by-concatenating-two-letter-words/)

###### Steps and Explanation:
1. **`longestPalindrome(String[] words)`**: This is the main method that invokes the `solve` method to compute the longest possible palindrome using the given array of words.   
2. **`solve(String[] words)`**: This method performs the core logic:
   - A `HashMap` is used to store the frequency of each word.
   - The loop goes through each word in the array. For each word, it checks:
     - If the word is not a palindrome (its reverse is different from the word), it checks if both the word and its reverse are present and uses them to form part of the palindrome.
     - If the word is a palindrome, it checks if two occurrences can be used as pairs, or, if only one is left, it uses it as the center of the palindrome (if no center has been used yet).   
3. **`reverseString(String s)`**: This helper method simply reverses the given string. It’s used to check if a word has a corresponding reverse in the array.

```java
class Solution {
    // Main function that calls the solve method to find the longest palindrome
    public int longestPalindrome(String[] words) {
        return solve(words);
    }

    // Helper function to calculate the longest palindrome that can be formed
    private int solve(String[] words) {
        // Create a HashMap to store the count of each word in the array
        Map<String, Integer> map = new HashMap<>();
        
        // Populate the map with the frequency of each word
        for (String s : words) {
            // If the word already exists in the map, increment its count, else set it to 1
            map.put(s, map.getOrDefault(s, 0) + 1);
        }

        // Variable to store the total length of the palindrome
        int res = 0;
        // Boolean flag to check if a word with two equal letters (like "aa") can be used as the center of the palindrome
        boolean centerUsed = false;

        // Loop through each word in the original array
        for (String word : words) {
            // Get the reversed version of the current word
            String rev = reverseString(word);

            // If the word is not a palindrome (the reverse is different from the word itself)
            if (!rev.equals(word)) {
                // Check if both the word and its reverse are present in the map
                if (map.get(word) > 0 && map.getOrDefault(rev, 0) > 0) {
                    // Use one occurrence of both the word and its reverse to form a part of the palindrome
                    map.put(word, map.get(word) - 1);
                    map.put(rev, map.get(rev) - 1);
                    // Since each pair contributes 4 characters (2 from the word and 2 from the reverse)
                    res += 4;
                }
            } 
            // If the word is a palindrome itself (like "aa", "bb")
            else {
                // If there are at least two occurrences of the palindrome word
                if (map.get(word) >= 2) {
                    // Use two occurrences of the palindrome word to form a part of the palindrome
                    map.put(word, map.get(word) - 2);
                    // Since two identical words contribute 4 characters
                    res += 4;
                }
                // If there is only one occurrence of the palindrome word and no center has been used yet
                else if (map.get(word) == 1 && !centerUsed) {
                    // Use this word as the center of the palindrome
                    map.put(word, map.get(word) - 1);
                    // This contributes 2 characters (since it's the center, it's counted once)
                    res += 2;
                    // Mark the center as used
                    centerUsed = true;
                }
            }
        }
        // Return the total length of the longest palindrome that can be formed
        return res;
    }

    // Helper function to reverse a string
    private String reverseString(String s) {
        // Use a StringBuilder to efficiently reverse the string
        StringBuilder res = new StringBuilder();
        // Iterate over the characters in reverse order and append them to the result
        for (int i = s.length() - 1; i >= 0; i--) {
            res.append(s.charAt(i));
        }
        // Convert the StringBuilder to a string and return it
        return res.toString();
    }
}
```

