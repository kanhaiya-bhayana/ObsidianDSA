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


## 8. [Maximum 69 Number](https://leetcode.com/problems/maximum-69-number/)

###### Step-by-step explanation:
1. The `maximum69Number` method calls the `solve` helper method to find the solution.
2. The `solve` method initializes variables: `placeValue` to track the position of each digit and `placeValueSix` to store the place value of the last '6' found.
3. It iterates over the digits of the number (`temp % 10` gives the last digit), and if the digit is '6', it updates `placeValueSix` with the current place value.
4. After the loop, if no '6' is found (i.e., `placeValueSix` remains -1), the method returns the original number.
5. If a '6' is found, the method adds `3 * (10^placeValueSix)` to change the first '6' to '9' at that place value and returns the modified number.

```java
class Solution {
    // The main method to find the maximum possible number by changing at most one '6' to '9'.
    public int maximum69Number(int num) {
        return solve(num);  // Call the helper function to solve the problem.
    }

    // Helper function to process the given number and replace the first '6' encountered with '9'.
    private int solve(int num) {
        int placeValue = 0;           // Tracks the current place value (ones, tens, hundreds, etc.)
        int placeValueSix = -1;        // Stores the place value of the last '6' encountered, initialized to -1 to indicate no '6' found yet.
        int temp = num;                // A temporary variable used to traverse through the digits of the number.

        // Loop through the digits of the number by continuously dividing by 10.
        while(temp != 0){
            int rem = temp % 10;       // Get the last digit of the current number.
            if (rem == 6){
                // If the current digit is '6', store its place value.
                placeValueSix = placeValue;
            }
            temp = temp / 10;          // Remove the last digit by integer division.
            placeValue++;              // Move to the next place value.
        }

        // If no '6' was found, return the original number.
        if (placeValueSix == -1){
            return num;
        }

        // Otherwise, change the '6' at the found place value to '9'.
        // To do this, add 3 * (10^placeValueSix) to the number (since 9 - 6 = 3).
        return num + 3 * (int)Math.pow(10, placeValueSix);
    }
}
```

## 9. [Maximum Bags With Full Capacity of Rocks](https://leetcode.com/problems/maximum-bags-with-full-capacity-of-rocks/)

###### Explanation of the Code:

1. **Input and Initialization**: 
   - The method takes three inputs: an array `capacity[]` representing the maximum capacity of each bag, an array `rocks[]` representing the current number of rocks in each bag, and an integer `additionalRocks` representing the total number of additional rocks we can distribute.
   
2. **Calculate Rocks Needed**:
   - For each bag, we compute how many more rocks are needed to fill it by subtracting the number of current rocks from the bag’s capacity (`requireRock[i] = capacity[i] - rocks[i]`).
   
3. **Sorting the Required Rocks**:
   - We sort the `requireRock[]` array in ascending order. This allows us to prioritize bags that require fewer rocks to be filled first, maximizing the number of completely filled bags.

4. **Filling Bags**:
   - We iterate through the sorted array of required rocks. If the number of additional rocks is enough to fill a bag, we fill it and decrease the number of available additional rocks. We continue this process until we run out of rocks or can no longer fill any more bags.

5. **Return the Result**:
   - The method returns the total number of bags that were completely filled.

```java
class Solution {
    public int maximumBags(int[] capacity, int[] rocks, int additionalRocks) {
        // Step 1: Determine the number of bags
        int n = rocks.length;
        
        // Step 2: Create an array to store the required number of rocks to fill each bag
        int[] requireRock = new int[n];
        
        // Step 3: Calculate the number of rocks required to fill each bag
        // For each bag, required rocks = capacity of the bag - current rocks in the bag
        for(int i = 0; i < n; i++) {
            requireRock[i] = capacity[i] - rocks[i];
        }
        
        // Step 4: Sort the requireRock array in ascending order
        // This allows us to prioritize filling bags that need fewer rocks first
        Arrays.sort(requireRock);
        
        // Step 5: Initialize a counter to track the number of completely filled bags
        int count = 0;
        
        // Step 6: Iterate through the sorted array of required rocks
        // Try to fill as many bags as possible with the available additional rocks
        for(int i = 0; i < n; i++) {
            // Step 7: Check if we have enough additional rocks to fill the current bag
            if(additionalRocks >= requireRock[i]) {
                // Step 8: Deduct the required rocks from the additional rocks
                additionalRocks -= requireRock[i];
                
                // Step 9: Increment the count of completely filled bags
                count++;
            } else {
                // Step 10: If we can't fill the current bag, stop the process
                break;
            }
        }
        
        // Step 11: Return the number of completely filled bags
        return count;
    }
}
```

## 10. [Minimum Rounds to Complete All Tasks](https://leetcode.com/problems/minimum-rounds-to-complete-all-tasks/)

###### Explanation:
1. **Problem Context**:
   - You are given a list of tasks, where each task has a difficulty level. You can only complete tasks in groups of 2 or 3 in each round.
   - The goal is to find the minimum number of rounds required to complete all tasks. If it's impossible (e.g., when a task appears only once, as it can't form a valid group), you return `-1`.

2. **Logic Breakdown**:
   - **Step 1**: First, count how many times each task difficulty appears using a `HashMap`. The key is the difficulty level, and the value is the count of tasks with that difficulty.
   - **Step 2**: For each task frequency:
     - If the count is `1`, return `-1` immediately because you can't form a group with just one task.
     - If the count is divisible by 3, you can complete those tasks using only groups of 3 rounds.
     - Otherwise, you'll need one additional round to handle the remainder after grouping by 3.
   - **Step 3**: Return the total number of rounds calculated.

This approach ensures the minimum number of rounds while handling edge cases like when a task appears only once.


```java
class Solution {
    // Public method that serves as the entry point of the solution.
    // It calls the helper method 'solve' to compute the minimum number of rounds.
    public int minimumRounds(int[] tasks) {
        return solve(tasks);
    }

    // Private helper method that computes the minimum number of rounds required to complete all tasks.
    private int solve(int[] tasks) {
        // Create a HashMap to store the frequency of each unique task.
        // The key is the task difficulty level, and the value is the count of how many times that task appears.
        Map<Integer, Integer> map = new HashMap<>();

        // Loop through the array of tasks to populate the frequency map.
        for (int n : tasks) {
            // For each task, increase its count in the map by 1.
            // If the task does not exist in the map yet, it is added with a default count of 1.
            map.put(n, map.getOrDefault(n, 0) + 1);
        }

        // Variable to keep track of the minimum number of rounds required.
        int cnt = 0;

        // Loop through the frequency values in the map (i.e., how many times each task appears).
        for (int n : map.values()) {
            // If the count of any task is 1, it is impossible to complete the task,
            // because you need at least two tasks to form a valid round (either 2 or 3 tasks per round).
            if (n == 1) {
                return -1; // Return -1 if a task cannot be completed.
            }

            // If the count is divisible by 3, we can divide the tasks into groups of 3 rounds.
            if (n % 3 == 0) {
                cnt += n / 3; // Add the number of groups of 3.
            } 
            // If the count is not divisible by 3, we form as many groups of 3 as possible,
            // then handle the remainder (which will be either 1 or 2) by adding an extra round.
            else {
                cnt += n / 3 + 1; // Add one more round to handle the remainder.
            }
        }

        // Return the total count of rounds needed.
        return cnt;
    }
}
```

## 11. [Maximum Ice Cream Bars](https://leetcode.com/problems/maximum-ice-cream-bars/)

###### Explanation of Steps:
1. **Sorting the costs**: Sorting ensures we can start by buying the cheapest ice creams first, maximizing the total number we can afford.
2. **Loop through sorted costs**: For each ice cream cost, we check if we still have enough coins. If not, we stop, as we can't afford any more ice creams.
3. **Buying the ice creams**: We subtract the cost from the available coins and increase the count of ice creams bought.
4. **Return the result**: After the loop, the method returns how many ice creams were bought.

This ensures an efficient solution, leveraging a greedy approach to maximize the number of ice creams bought.

```java
import java.util.Arrays;

class Solution {
    public int maxIceCream(int[] costs, int coins) {
        // Step 1: Sort the array of ice cream costs in ascending order.
        // This helps in buying the cheaper ice creams first to maximize the count.
        Arrays.sort(costs);
        
        int cnt = 0; // Counter to keep track of how many ice creams can be bought.
        
        // Step 2: Loop through the sorted costs array and try to buy as many ice creams as possible.
        for (int cost : costs) {
            // Step 3: If the current cost is greater than the remaining coins, exit the loop.
            // No more ice creams can be bought.
            if (cost > coins) {
                break;
            }
            // Step 4: Subtract the cost of the current ice cream from the available coins.
            coins -= cost;
            // Step 5: Increment the count of ice creams bought.
            cnt++;
        }
        
        // Step 6: Return the total number of ice creams bought.
        return cnt;
    }
}
```

## 12. [Gas Station](https://leetcode.com/problems/gas-station/)

###### Explanation:
1. **Method Signature:**
   - `canCompleteCircuit(int[] gas, int[] cost)`: This method aims to find the starting gas station index from which you can complete the circuit around all gas stations or return `-1` if it's not possible.

2. **Check Feasibility:**
   - `int allGas = sumAll(gas)` and `int allCost = sumAll(cost)` calculate the total gas available and total cost to travel between all stations.
   - If the total gas is less than the total cost, it’s impossible to complete the circuit, and the method immediately returns `-1`.

3. **Tracking Variables:**
   - `n = gas.length`: The number of gas stations.
   - `total`: A variable used to track the running balance of gas. This is adjusted by adding the gas gained and subtracting the cost to travel to the next station.
   - `res`: Tracks the starting index of the station. It gets updated whenever we encounter a negative balance in the `total`.

4. **Iterating Through Gas Stations:**
   - For each gas station, the balance `total` is updated by calculating `gas[i] - cost[i]`.
   - If `total` becomes negative, it means that starting from the previous `res` wouldn't allow completing the circuit, so the starting station (`res`) is updated to the next station (`i + 1`), and `total` is reset to zero.

5. **Helper Method:**
   - `sumAll(int[] arr)` calculates the total of all elements in an array. This method is used to sum the gas and cost arrays at the beginning of the algorithm.

By the end of the loop, the variable `res` will point to the gas station from which you can complete the circuit, if possible.

```java
class Solution {
    
    // Method to determine the starting gas station from which we can complete the circuit
    public int canCompleteCircuit(int[] gas, int[] cost) {
        // Calculate the total amount of gas and cost across all stations
        int allGas = sumAll(gas);  // Total gas available at all stations
        int allCost = sumAll(cost);  // Total cost to travel between all stations
        
        // If total gas is less than total cost, it's impossible to complete the circuit
        if (allGas < allCost){
            return -1;  // Return -1 indicating no valid starting point
        }

        // Initialize variables for tracking the current total and the starting station
        int n = gas.length;  // Number of gas stations
        int total = 0;  // Tracks the running gas balance while iterating through stations
        int res = 0;  // The index of the gas station to start from
        
        // Iterate over each gas station
        for (int i = 0; i < n; i++) {
            // Update the current gas balance: gas gained - cost to move to the next station
            total += gas[i] - cost[i];
            
            // If at any point the balance becomes negative, reset and try starting from the next station
            if (total < 0) {
                res = i + 1;  // Update the starting station to the next one
                total = 0;  // Reset the total balance to zero
            }
        }
        
        // Return the index of the starting station from which the circuit can be completed
        return res;
    }

    // Helper method to calculate the sum of all elements in an array
    private int sumAll(int[] arr) {
        int sum = 0;  // Initialize sum to zero
        // Loop through each element and add it to the sum
        for (int n : arr) {
            sum += n;
        }
        return sum;  // Return the total sum
    }
}
```

## 13. [Optimal Partition of String](https://leetcode.com/problems/optimal-partition-of-string/)

###### Explanation:

1. **Initialization**:
   - The `arr` array keeps track of the last seen index of each character. Since there are 26 lowercase English letters, the size of the array is 26. We initialize all elements to `-1` because initially, none of the characters have been seen.
   - `count` is used to keep track of how many partitions are made.
   - `currStart` represents the start index of the current substring (partition).

2. **Iterating through the string**:
   - For each character in the string, we check whether that character has already appeared in the current partition by comparing its last seen index (`arr[s.charAt(i) - 'a']`) with `currStart`.
   - If the last occurrence is within the current partition (`>= currStart`), a new partition is started, the count is incremented, and `currStart` is updated to the current position (`i`).

3. **Update the last occurrence**:
   - After checking, we update the last occurrence of the current character in the `arr` array to the current index `i`.

4. **Final partition**:
   - Since the loop doesn't count the last partition, we return `count + 1` to include it.

This approach ensures that no character repeats within the same partition, leading to an optimal number of partitions.

```java
class Solution {
    public int partitionString(String s) {
        // Initialize an array to keep track of the last occurrence index of each character in the string
        // Since there are 26 lowercase English letters, we use an array of size 26
        // We fill the array with -1 initially to indicate that no character has been encountered yet
        int[] arr = new int[26];
        Arrays.fill(arr, -1);  // Fill the array with -1 to mark unvisited characters

        int count = 0;         // Variable to count the number of partitions
        int currStart = 0;     // Variable to keep track of the start index of the current partition

        // Iterate through the string character by character
        for (int i = 0; i < s.length(); i++) {
            // If the character at index 'i' has appeared after the current partition started,
            // we need to start a new partition
            if (arr[s.charAt(i) - 'a'] >= currStart) {
                count++;        // Increment the partition count
                currStart = i;  // Update the current start index to the new partition's starting point
            }

            // Update the last seen index of the current character
            arr[s.charAt(i) - 'a'] = i;
        }

        // Return the total number of partitions
        // We add 1 to 'count' since the last partition is not included in the loop count
        return count + 1;
    }
}
```

