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

## 14. [Dota2 Senate](https://leetcode.com/problems/dota2-senate/)

## 15. [Minimum Deletions to Make Character Frequencies Unique](https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique/)

###### 
Breakdown:
1. **Frequency Calculation**: The first loop calculates the frequency of each character in the input string.
2. **Sorting**: Sorting the frequency array helps process the characters with higher frequencies first and ensures the unique frequency requirement is easier to enforce.
3. **Adjusting Frequencies**: The second loop ensures that each frequency is strictly less than the next frequency by reducing it if needed, counting the number of deletions required.
4. **Result Calculation**: The `result` variable keeps track of how many deletions are required in total to make all character frequencies unique.

```java
class Solution {
    public int minDeletions(String s) {
        // Step 1: Initialize an array to store the frequency of each character in the string.
        // The size is 26 to account for each lowercase English letter ('a' to 'z').
        int[] freq = new int[26];

        // Step 2: Iterate through each character in the string and update the frequency array.
        // For each character, we calculate its index by subtracting 'a' to get a zero-based index.
        for (char ch : s.toCharArray()) {
            freq[ch - 'a']++;
        }

        // Step 3: Sort the frequency array in ascending order to make it easier to manage
        // characters with higher frequencies and ensure that we can process larger frequencies first.
        Arrays.sort(freq);

        // Step 4: Initialize a variable to keep track of the minimum number of deletions needed.
        int result = 0;

        // Step 5: Loop through the sorted frequency array starting from the second highest value (index 24).
        // We go backward and compare each frequency with the one after it (to ensure all frequencies are unique).
        for (int i = 24; i >= 0 && freq[i] > 0; i--) {
            // Step 6: If the current frequency is greater than or equal to the next frequency,
            // we need to reduce the current frequency to ensure it's smaller than the next one.
            if (freq[i] >= freq[i + 1]) {
                // Store the current frequency before modification for calculation.
                int prev = freq[i];
                // Reduce the current frequency to be one less than the next frequency or 0 if needed.
                freq[i] = Math.max(0, freq[i + 1] - 1);
                // Increment the result by the difference, which represents the number of deletions made.
                result += prev - freq[i];
            }
        }

        // Step 7: Return the total number of deletions required to ensure all character frequencies are unique.
        return result;
    }
}
```

## 16. [Candy](https://leetcode.com/problems/candy/)
###### Breakdown:
1. **Initialization**: 
   - The number of children is determined by the length of the `ratings` array.
   - An array `count` is initialized to store the number of candies for each child, and each child is given at least one candy initially.

2. **First Pass (Left to Right)**:
   - The code checks from left to right to ensure that if a child has a higher rating than the previous child, they should receive more candies. 
   - If the current child has a higher rating than the previous child, their candy count is increased by 1 more than the previous child.

3. **Second Pass (Right to Left)**:
   - The code checks from right to left to ensure that if a child has a higher rating than the next child, they should receive more candies. 
   - In this step, the result from the first pass is considered to avoid decreasing the number of candies already assigned in the left-to-right pass.

4. **Summing the Results**:
   - After adjusting the candies in both passes, the total number of candies is calculated by summing the values in the `count` array.

5. **Final Result**:
   - The total number of candies required is returned as the result.

```java
class Solution {
    public int candy(int[] ratings) {
        // Step 1: Get the number of children.
        int n = ratings.length;

        // Step 2: Initialize an array to store the number of candies each child gets.
        // Initially, each child gets 1 candy.
        int[] count = new int[n];
        Arrays.fill(count, 1);

        // Step 3: Traverse the ratings from left to right. If the current child's rating
        // is higher than the previous child's rating, give them one more candy than the previous child.
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                count[i] = Math.max(count[i], count[i - 1] + 1);
            }
        }

        // Step 4: Traverse the ratings from right to left. If the current child's rating
        // is higher than the next child's rating, give them one more candy than the next child.
        // However, ensure that we don't reduce the candy count from the first traversal.
        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                count[i] = Math.max(count[i], count[i + 1] + 1);
            }
        }

        // Step 5: Calculate the total number of candies by summing up the values in the count array.
        int res = 0;
        for (int c : count) {
            res += c;
        }

        // Step 6: Return the total number of candies needed.
        return res;
    }
}
```

## 17. [Remove Colored Pieces if Both Neighbors are the Same Color](https://leetcode.com/problems/remove-colored-pieces-if-both-neighbors-are-the-same-color/)

###### Explanation:
1. **Initialization of Counters**: Two variables `a` and `b` are used to count how many sets of 3 consecutive 'A's and 'B's appear, respectively.
   
2. **Two-pointer Traversal**: The code uses two pointers `i` and `j`, where `j` is always 2 positions ahead of `i`, allowing the code to examine 3 consecutive characters in each loop iteration.

3. **Consecutive Check**: For every set of 3 consecutive characters (starting at `i` and ending at `j`), the code checks if all the characters are the same. If so, it increments the respective counter for 'A' or 'B'.

4. **Determining the Winner**: After iterating through the string, the code compares the counts of 'A' and 'B'. If `a` (the number of consecutive 'A's) is greater than `b`, player A is the winner. Otherwise, player B is the winner or the result is a tie.

```java
class Solution {
    public boolean winnerOfGame(String c) {
        // Initialize counters for 'A' and 'B' consecutive moves
        int a = 0; // Tracks consecutive 'A's
        int b = 0; // Tracks consecutive 'B's

        // Store the length of the input string
        int n = c.length();
        
        // Initialize two pointers, i for current char, j for the char two positions ahead
        int i = 0;
        int j = i + 2; // j is always 2 characters ahead of i to check 3 consecutive chars

        // Iterate through the string while ensuring j is within bounds
        while (j < n) {
            // Get the current char and the next one (i+1) in the string
            char first = c.charAt(i);
            char second = c.charAt(i + 1);

            // Check if the three consecutive characters are the same
            if (first == second && first == c.charAt(j)) {
                // If all 3 are 'A', increment 'a' counter
                if (first == 'A') {
                    a++;
                // If all 3 are 'B', increment 'b' counter
                } else if (first == 'B') {
                    b++;
                }
            }
            // Move both pointers one step ahead to check the next set of 3 characters
            i++;
            j++;
        }

        // Return true if player A has more moves than player B, otherwise return false
        return a > b;
    }
}
```

## 18. [Maximum Score of a Good Subarray](https://leetcode.com/problems/maximum-score-of-a-good-subarray/)

###### Explanation:
1. **Initial Setup:** The problem requires finding the maximum score of a subarray that includes the element at index `k`. The score of a subarray is defined as the minimum value of the subarray multiplied by its length.
   
2. **Two Pointers:** The solution uses two pointers, `i` and `j`, which start at `k` and are expanded outward (left or right) to cover larger subarrays.

3. **Minimum Value Maintenance:** As the pointers expand, the current minimum value (`currMin`) in the subarray is updated by taking the minimum value between the previous `currMin` and the newly added element (either from the left or right).

4. **Score Calculation:** For each expanded subarray, the score is calculated as `currMin * (j - i + 1)` and the result is updated with the maximum score found so far.

5. **Termination Condition:** The expansion continues until both pointers can no longer move (i.e., `i` cannot move left and `j` cannot move right).

```java
class Solution {
    public int maximumScore(int[] nums, int k) {
        
        // Step 1: Initialize the length of the array.
        int n = nums.length;

        // Step 2: Set up two pointers 'i' and 'j' to start from the index 'k'. 
        // These pointers will expand left and right to explore subarrays around 'k'.
        int i = k;
        int j = k;

        // Step 3: Initialize 'currMin' to the value at index 'k'.
        // This keeps track of the minimum value in the current subarray.
        int currMin = nums[k];

        // Step 4: Initialize the result with the value at 'k', which is the initial subarray of size 1.
        int result = nums[k];

        // Step 5: Start expanding the subarray by moving the pointers 'i' and 'j' outward.
        // We keep expanding as long as 'i' can move left or 'j' can move right.
        while (i > 0 || j < n - 1) {
            
            // Step 6: Calculate the potential values from the left and right to decide the direction of expansion.
            // If 'i' can move left, get 'leftValue', otherwise set it to 0.
            int leftValue = (i > 0) ? nums[i - 1] : 0;

            // If 'j' can move right, get 'rightValue', otherwise set it to 0.
            int rightValue = (j < n - 1) ? nums[j + 1] : 0;

            // Step 7: Compare the left and right values. Move the pointer that corresponds to the larger value.
            // If the right value is greater, expand right by incrementing 'j'.
            if (leftValue < rightValue) {
                j++;
                
                // Update 'currMin' to the minimum value in the current expanded subarray.
                currMin = Math.min(currMin, nums[j]);
            }
            // Otherwise, expand left by decrementing 'i'.
            else {
                i--;

                // Update 'currMin' to the minimum value in the current expanded subarray.
                currMin = Math.min(currMin, nums[i]);
            }

            // Step 8: Calculate the score for the current subarray (from index 'i' to 'j').
            // The score is the minimum value in the subarray multiplied by its length (j - i + 1).
            result = Math.max(result, currMin * (j - i + 1));
        }

        // Step 9: Return the maximum score obtained.
        return result;
    }
}
```

## 19. [Eliminate Maximum Number of Monsters](https://leetcode.com/problems/eliminate-maximum-number-of-monsters/)

###### Explanation:
1. **Initialization**:
   - An array `time[]` is created to store the time required for each monster to reach the city.
   - `count` is initialized to `1` because the first monster can always be eliminated at time 0.
   
2. **Calculate time for each monster**:
   - The loop iterates over each monster to compute the time it takes to reach the city (`dist[i] / speed[i]`), using `Math.ceil` to handle fractional times.

3. **Sort the arrival times**:
   - After calculating the arrival times, the array is sorted so that we can eliminate the monsters in order of when they arrive.

4. **Loop through the monsters**:
   - The second loop goes through the sorted `time[]` array. 
   - If at any point, a monster reaches the city before the current time (`time[i] - timePassed <= 0`), we return the current `count` as we can no longer eliminate it in time.
   - Otherwise, the monster is eliminated, and we increment both the `count` of eliminated monsters and the `timePassed`.

5. **Return the result**:
   - If all monsters are eliminated successfully before any reach the city, the total `count` is returned.

```java
class Solution {
    public int eliminateMaximum(int[] dist, int[] speed) {
        // Create an array 'time' to store the time taken for each monster to reach the city
        int[] time = new int[dist.length];

        // Get the number of monsters
        int n = dist.length;
        
        // Initialize count to 1 because the first monster is always eliminated at time 0
        int count = 1;

        // Calculate the time each monster takes to reach the city using dist[i] / speed[i]
        // We use Math.ceil to round up to the nearest integer since even partial progress counts
        for (int i = 0; i < n; i++) {
            time[i] = (int) Math.ceil((double) dist[i] / speed[i]);
        }

        // Sort the time array in ascending order
        // This allows us to deal with the monsters in the order they arrive at the city
        Arrays.sort(time);

        // Variable to keep track of how much time has passed (starts at 1 as the first action happens at time 1)
        int timePassed = 1;

        // Loop through the remaining monsters (starting from the second one)
        for (int i = 1; i < n; i++) {
            // If the current monster arrives before or at the same time as the time passed, return the count
            // This means the monster arrives before you can eliminate it
            if (time[i] - timePassed <= 0) {
                return count;
            }

            // Increment count for the number of monsters successfully eliminated
            count++;

            // Increment the time passed since each elimination takes 1 unit of time
            timePassed += 1;
        }

        // Return the total number of monsters eliminated if all were handled before reaching the city
        return count;
    }
}
```

## 20. [Maximum Element After Decreasing and Rearranging](https://leetcode.com/problems/maximum-element-after-decreasing-and-rearranging/)

###### Explanation Steps:
1. **Sorting the Array**: Sorting ensures that all elements are processed in ascending order, which makes it easier to achieve the requirement that values are consecutive from `1` upwards.
  
2. **Initializing `ans` to 1**: We start `ans` at 1, as this is the minimum value we aim to reach after arranging the elements to maintain the order constraint.

3. **Looping Through the Array**:
   - Starting from the second element (`i = 1`), check if the current element `arr[i]` can support incrementing `ans` to maintain consecutive values.
   - If `arr[i]` is greater than or equal to `ans + 1`, increment `ans`.
   - If `arr[i]` is less than `ans + 1`, skip incrementing to preserve the consecutive sequence without forcing a decrement.

4. **Returning the Result**: After the loop, `ans` will hold the maximum element achievable while maintaining the consecutive order starting from `1`.

```java
import java.util.Arrays;

class Solution {
    public int maximumElementAfterDecrementingAndRearranging(int[] arr) {
        // Sort the array in ascending order to ensure we start with the smallest element first
        Arrays.sort(arr);

        // Initialize `ans` to 1 because the smallest possible value we can get is 1
        int ans = 1;

        // Traverse the sorted array starting from the second element
        for (int i = 1; i < arr.length; i++) {
            // Check if the current element `arr[i]` is at least `ans + 1`
            // This ensures that we can increment `ans` to maintain consecutive values
            if (arr[i] >= ans + 1) {
                ans++; // Increment `ans` to get the next maximum possible value
            }
            // If arr[i] < ans + 1, we leave `ans` as is to maintain consecutive order
        }

        // Return the maximum element we can achieve after rearrangement and decrementing
        return ans;
    }
}
```

## 21. [Maximum Number of Coins You Can Get](https://leetcode.com/problems/maximum-number-of-coins-you-can-get/)

###### Explanation Steps:

1. **Sorting the Array**: Sorting ensures that the piles are in ascending order, which helps in efficiently selecting piles based on size.

2. **Setting Up Pointers**:
   - `i` starts from the beginning (smallest pile) and will advance one step each iteration to skip the smallest pile.
   - `j` starts from the end (largest pile) and will decrement by 2 each iteration to skip both the largest pile and include the middle pile (the optimal pile we can collect).
   - `m` is initialized to accumulate the sum of coins from the selected middle pile in each turn.

3. **Looping Through the Piles**:
   - In each iteration, `i++` skips the smallest pile from the current selection of three piles.
   - `m += piles[j - 1]` adds the middle pile’s coins to the total.
   - `j -= 2` skips the largest pile and moves to the next set of three piles.
   
4. **Returning the Result**: After the loop completes, `m` holds the maximum coins that can be collected based on this strategy.

```java
import java.util.Arrays;

class Solution {
    public int maxCoins(int[] piles) {
        // Sort the array in ascending order to easily pick the largest values
        Arrays.sort(piles);

        // Initialize two pointers: `i` starts from the beginning, `j` from the end
        int i = 0; // This will represent the lowest pile in each turn
        int j = piles.length - 1; // This will represent the highest pile in each turn
        int m = 0; // This will hold the sum of coins we collect

        // Continue until `i` meets `j` (we've processed all piles)
        while (i < j) {
            i++; // Move `i` one step forward (skip the smallest pile)
            m += piles[j - 1]; // Add the middle pile (the maximum coins we can pick)
            j -= 2; // Move `j` two steps back to exclude the current largest and middle piles
        }

        // Return the total maximum coins collected
        return m;
    }
}
```

## 22. [Minimum Number of Operations to Make Array Empty](https://leetcode.com/problems/minimum-number-of-operations-to-make-array-empty/)

###### Explanation Steps:

1. **Initialize Frequency Map**:
   - Use a `HashMap` to count how often each number appears in the `nums` array. This allows you to quickly determine how many operations are needed for each unique number.

2. **Populate the Frequency Map**:
   - For each number `n` in `nums`, increment its count in the map using `map.put(n, map.getOrDefault(n, 0) + 1)`, which initializes to 0 if `n` is not already in the map.

3. **Initialize Result Variable**:
   - `res` is initialized to hold the total minimum operations needed.

4. **Iterate Through the Map Entries**:
   - For each unique number and its frequency (`ele.getValue()`):
     - If the frequency is `1`, it's impossible to group it into pairs or triples, so return `-1`.
     - Otherwise, calculate the minimum number of operations needed by dividing the frequency by `3` and rounding up with `Math.ceil`. This is done to either form groups of 3, or handle leftover pairs.

5. **Return the Result**:
   - After processing all unique numbers, `res` will contain the minimum number of operations needed to satisfy the grouping requirement.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int minOperations(int[] nums) {
        // Create a map to count the frequency of each number in the array
        Map<Integer, Integer> map = new HashMap<>();

        // Populate the map with counts of each number in nums
        for (int n : nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
        }

        int res = 0; // This will store the total minimum operations needed

        // Iterate through each unique number and its count in the map
        for (Map.Entry<Integer, Integer> ele : map.entrySet()) {
            // If the frequency of a number is 1, it's impossible to form groups of 2 or more
            // So, return -1 to indicate it's not possible to achieve the target
            if (ele.getValue() == 1) {
                return -1;
            }

            // Calculate the minimum operations to reduce the count of this number to groups of 2 or 3
            // Math.ceil(ele.getValue() / 3) gives the required groups of 3, or adjusts for leftover count 2
            res += Math.ceil((double) ele.getValue() / 3.0);
        }

        // Return the total operations required to make all numbers into groups of 2 or 3
        return res;
    }
}
```

## 23. [Find Polygon With the Largest Perimeter](https://leetcode.com/problems/find-polygon-with-the-largest-perimeter/)

###### Explanation Steps:

1. **Sorting the Array**:
   - Sorting the array helps to access the largest elements easily, as larger sides are more likely to form a valid triangle perimeter with neighboring elements.

2. **Initializing Variables**:
   - `res` is initialized to `-1` to indicate no valid triangle has been found initially.
   - `total` is initialized to `0` and will store the sum of two largest side lengths as we iterate.

3. **Iterating Through the Array**:
   - For each element `e` in `nums`, we check if the current `total` can form a valid triangle with `e`. In a valid triangle, the sum of any two sides must be greater than the third side.
   - If `total > e`, then `res` is updated to `total + e`, representing the perimeter of a valid triangle.
   - Regardless of validity, `e` is added to `total` to track the sum of sides for potential future checks.

4. **Returning the Result**:
   - After the loop, `res` contains the largest valid perimeter found. If no valid triangle was formed, `res` remains `-1`.

```java
import java.util.Arrays;

class Solution {
    public long largestPerimeter(int[] nums) {
        // Sort the array in ascending order to try forming the largest perimeter with the largest elements
        Arrays.sort(nums);

        long res = -1; // Initialize `res` with -1 to represent no valid perimeter initially
        long total = 0; // `total` will store the sum of the two largest side lengths as we iterate

        // Iterate through each element in the sorted array
        for (int e : nums) {
            // Check if the current value of `total` can form a valid triangle with `e`
            // For a valid triangle, the sum of two sides must be greater than the third side
            if (total > e) {
                res = total + e; // Update `res` with the current perimeter if it’s valid
            }
            // Add the current element `e` to `total` to keep track of the largest sides as we iterate
            total += e;
        }

        // Return the largest valid perimeter found, or -1 if no valid triangle can be formed
        return res;
    }
}
```

## 24. [Least Number of Unique Integers after K Removals](https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/)
###### Explanation Steps:

1. **Building the Frequency Map**:
   - A `HashMap` is used to store the frequency of each integer in `arr`, with each integer as a key and its count as the value.

2. **Initializing the Priority Queue**:
   - A `PriorityQueue` (min-heap) is created to store the frequencies in ascending order. This allows easy access to the least frequent elements, which are removed first to minimize the number of unique integers.

3. **Adding Frequencies to the Priority Queue**:
   - All frequencies from the map are added to the priority queue. This queue will prioritize smaller frequencies, making it efficient to target the least frequent elements first for removal.

4. **Reducing Elements by Frequency**:
   - The loop removes elements from the queue based on the frequency counts until `k` elements are removed:
     - `q.poll()` retrieves the smallest frequency element.
     - If the frequency is greater than `1`, it’s decremented and reinserted, as there are still occurrences left.
     - `k` is decremented with each removal.

5. **Calculating the Result**:
   - After the loop, the queue’s size represents the remaining unique integers that couldn’t be removed, so `q.size()` is returned.

```java
import java.util.HashMap;
import java.util.Map;
import java.util.PriorityQueue;
import java.util.Queue;

class Solution {

    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        // Create a map to store the frequency of each integer in the array
        Map<Integer, Integer> map = new HashMap<>();
        int res = 0; // `res` will hold the count of unique integers left after removing `k` elements

        // Populate the frequency map
        for (int a : arr) {
            map.put(a, map.getOrDefault(a, 0) + 1);
        }

        // Priority queue to store frequencies in ascending order (to remove least frequent elements first)
        Queue<Integer> q = new PriorityQueue<>();

        // Add all frequencies from the map into the priority queue
        for (Map.Entry<Integer, Integer> mp : map.entrySet()) {
            int ele = mp.getValue();
            if (ele >= 1) { // Only consider elements that appear at least once
                q.offer(ele);
            }
        }

        // Remove elements from the queue until `k` elements are removed or the queue is empty
        while (k != 0 && !q.isEmpty()) {
            int val = q.poll(); // Poll the least frequent element
            if (val >= 2) {
                q.offer(--val); // Decrement its count and reinsert if it's still present
            }
            k--; // Decrement `k` with each removal
        }

        // The remaining size of the queue represents the unique integers left
        return q.size();
    }
}
```

## 25. [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)
###### Explanation Steps:

1. **Initialize a Max-Heap**:
   - A `PriorityQueue` (`q`) with a custom comparator `(a, b) -> b - a` is used as a max-heap to keep track of the largest jumps made using bricks, allowing us to reclaim the largest brick usage if we need a ladder later.

2. **Loop Through Each Building**:
   - For each building at index `i`, check if a jump is needed to reach `heights[i + 1]`.
     - If `heights[i] >= heights[i + 1]`, no jump is needed, so continue to the next building.
     - If `heights[i + 1]` is higher, calculate `bricksUsed` as the difference in height.

3. **Deduct Bricks and Record Usage**:
   - Subtract `bricksUsed` from `bricks`.
   - Add `bricksUsed` to the max-heap `q` to track it in case we need to reclaim it with a ladder.

4. **Check for Insufficient Bricks**:
   - If `bricks` becomes negative (insufficient bricks for the current jump):
     - Use the largest bricks-used value by polling `q` to reclaim bricks.
     - If ladders are available, decrement `ladders` and continue; otherwise, return the current building index `i` as the farthest reachable.

5. **Return the Result**:
   - If all buildings are reachable, return `heights.length - 1`, indicating that we successfully reached the last building.

```java
import java.util.PriorityQueue;
import java.util.Queue;

class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        // Max-heap to store the bricks used for the largest jumps
        Queue<Integer> q = new PriorityQueue<>((a, b) -> b - a);

        // Loop through each building from the start to the second last building
        for (int i = 0; i < heights.length - 1; i++) {
            // Check if the next building is higher
            if (heights[i] >= heights[i + 1]) {
                continue; // Move to the next building if no jump is needed
            }

            // Calculate the number of bricks required to reach the next building
            int bricksUsed = heights[i + 1] - heights[i];
            bricks -= bricksUsed; // Deduct the used bricks from the total count
            q.offer(bricksUsed); // Add the bricks used to the max-heap

            // Check if bricks are insufficient (negative balance)
            if (bricks < 0) {
                // Reclaim the largest bricksUsed by adding back the largest jump stored in the max-heap
                bricks += q.poll();

                // Use a ladder if available, otherwise return the current building index
                if (ladders > 0) {
                    ladders--;
                } else {
                    return i; // Return the farthest reachable building index
                }
            }
        }
        // If all buildings are reachable, return the last building index
        return heights.length - 1;
    }
}
```

## 26. [Earliest Second to Mark Indices I](https://leetcode.com/problems/earliest-second-to-mark-indices-i/)
###### Explanation Steps:

1. **Adjust `changeIndices` to Zero-Based**:
   - The `changeIndices` array is modified to be zero-based for easier access within array indexing.

2. **Helper Method (`helper`)**:
   - **Purpose**: Check if all indices in `nums` can be marked by a given time `idx`.
   - **Logic**: 
     - Uses a `boolean` array `vis` to keep track of visited indices, and `visCnt` to count the unique marked indices.
     - As it iterates from `idx` backward:
       - If an index (`validx`) is already visited, it decrements `des` if needed.
       - If not, it marks the index as visited, increments `visCnt`, and adjusts `des` using the `nums` array.
     - Returns true if all indices are marked and `des` reaches zero.

3. **Binary Search to Find Earliest Valid Second**:
   - Checks if all indices can be marked by the last second in `changeIndices` using `helper`.
   - If possible, performs a binary search over `changeIndices`:
     - Uses `helper` to check each midpoint (`mid`) and adjusts `low` and `high` until the earliest valid time is found.
   - Returns `high + 1` (second index) as the earliest time where marking is feasible.

```java
class Solution {
    // Helper method to check if all indices can be marked by a specific time `idx`
    private boolean helper(int[] nums, int[] changeIndices, int idx) {
        boolean[] vis = new boolean[nums.length]; // Track visited indices
        int visCnt = 0; // Count of unique indices visited
        int des = 0; // Track the current desired state value

        // Loop through indices up to `idx` to simulate the marking process
        for (int i = idx; i >= 0; i--) {
            int validx = changeIndices[i]; // Get the index to be marked at this step
            
            // Check if this index has already been marked
            if (vis[validx]) {
                if (des > 0) {
                    des--; // Decrement desired count if this index was marked before
                }
            } else {
                vis[validx] = true; // Mark this index as visited
                visCnt++; // Increment unique visited count
                des += nums[validx]; // Adjust desired state based on nums value at this index
            }
        }
        
        // Check if all indices have been marked (`visCnt == nums.length`) and `des` is zero
        return des == 0 && visCnt == nums.length;
    }

    // Main method to find the earliest second where all indices can be marked as required
    public int earliestSecondToMarkIndices(int[] nums, int[] changeIndices) {
        // Decrement each element in `changeIndices` to make them zero-based for array indexing
        for (int i = 0; i < changeIndices.length; i++) {
            changeIndices[i]--;
        }

        // Check if it's possible to mark all indices up to the last time in `changeIndices`
        if (!helper(nums, changeIndices, changeIndices.length - 1)) {
            return -1; // Return -1 if it’s not possible to mark all indices
        }

        // Binary search to find the minimum `idx` where marking all indices is feasible
        int low = 0, high = changeIndices.length - 1;
        while (low < high) {
            int mid = (high - low) / 2 + low; // Calculate midpoint
            
            // If `helper` returns true for `mid`, reduce the range to find an earlier valid index
            if (helper(nums, changeIndices, mid)) {
                high = mid;
            } else {
                low = mid + 1; // Otherwise, adjust range to search later indices
            }
        }
        
        // Return `high + 1` as the earliest second (convert index to second)
        return high + 1;
    }
}
```

## 27. [Minimum Deletions to Make String K-Special](https://leetcode.com/problems/minimum-deletions-to-make-string-k-special/)
###### Explanation Steps:

1. **Calculate Character Frequencies**:
   - The code counts the frequency of each character in the input string `word` and stores them in the `freq` array, where each index represents a character ('a' to 'z').

2. **Sort Frequency Array**:
   - Sorting the `freq` array allows the algorithm to manage deletions in increasing order of frequency, making it easier to minimize deletions.

3. **Outer Loop for Minimum Frequency**:
   - Iterates over each possible minimum frequency `minFreq` in the sorted array.
   - `temp` tracks the deletions required if `minFreq` is considered the minimum allowed frequency for characters.

4. **Inner Loop to Adjust Higher Frequencies**:
   - For each `minFreq`, the inner loop iterates from the highest frequencies (end of the array) down to the current index `i`.
   - If the difference between `freq[j]` and `minFreq` exceeds `k`, additional deletions are needed to reduce `freq[j]` to a manageable level.
   - These deletions are accumulated in `temp`.

5. **Update Minimum Deletions**:
   - `result` is updated to the minimum deletions found across all possible values of `minFreq`.

6. **Accumulate Deletions for Next Iteration**:
   - `deletedTillNow` keeps track of total deletions up to the current frequency level, contributing to the deletion count in subsequent iterations.

7. **Return Result**:
   - Finally, the minimum number of deletions needed to satisfy the frequency condition for the entire string is returned.

```java
class Solution {
    public int minimumDeletions(String word, int k) {
        int[] freq = new int[26]; // Array to store frequency of each character ('a' to 'z')

        // Count frequency of each character in the given string
        for(char ch : word.toCharArray()) {
            freq[ch - 'a']++; // Increment frequency based on ASCII offset for 'a'
        }

        // Sort frequencies in ascending order to manage deletions starting from least to most frequent
        Arrays.sort(freq);

        int result = Integer.MAX_VALUE; // Initialize result with maximum possible value
        int deletedTillNow = 0; // Track total deletions performed so far

        // Iterate through each frequency to evaluate minimum deletions needed
        for(int i = 0; i < 26; i++) {
            int minFreq = freq[i]; // Current minimum frequency in sorted list
            int temp = deletedTillNow; // Temporary variable to track deletions for this iteration

            // Iterate from the end of frequency array down to the current index
            for(int j = 25; j > i; j--) {
                // If difference between current max frequency and minFreq is within allowed range, stop
                if(freq[j] - freq[i] <= k) 
                    break;
                
                // Otherwise, add deletions needed to bring `freq[j]` down to the acceptable difference `k`
                temp += freq[j] - minFreq - k;
            }

            // Update result to reflect the minimum deletions required so far
            result = Math.min(result, temp);

            // Accumulate total deletions till current position
            deletedTillNow += minFreq;
        }

        // Return the minimum deletions needed to satisfy the conditions
        return result;
    }
}
```

## 28. [Task Scheduler](https://leetcode.com/problems/task-scheduler/)
###### Explanation Steps:

1. **Check for No Cooldown**:
   - If `n == 0`, there’s no cooldown period between tasks, so the minimum time required is simply the length of the `tasks` array.

2. **Calculate Task Frequencies**:
   - Count the frequency of each task using an array `taskFreq` where each index represents a letter from 'A' to 'Z'.

3. **Sort Task Frequencies**:
   - Sorting `taskFreq` helps in identifying the task with the highest frequency, located at `taskFreq[25]` after sorting.

4. **Calculate Idle Slots**:
   - `batch` holds the frequency of the most frequent task.
   - `idleSlots` is calculated based on `(batch - 1) * n`, which represents the spaces between `batch - 1` occurrences of the most frequent task, where each space can accommodate `n` idle slots.

5. **Reduce Idle Slots with Other Tasks**:
   - The `for` loop iterates over the other task frequencies to reduce idle slots.
   - `Math.min(taskFreq[i], batch - 1)` ensures we don’t overfill slots with tasks that have lower frequencies.

6. **Return Total Interval**:
   - If there are idle slots remaining after filling with other tasks, return `idleSlots + tasks.length` (total tasks plus idle time).
   - If no idle slots are left, return only the length of `tasks` since no additional idle time is needed.

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        // If there is no cooldown time (n = 0), just return the total number of tasks
        if (n == 0) {
            return tasks.length;
        }

        // Array to store frequency of each task (assuming tasks are represented by uppercase letters A-Z)
        int[] taskFreq = new int[26];
        for (char c : tasks) {
            taskFreq[c - 'A']++; // Count each task's occurrences
        }

        // Sort task frequencies in ascending order to easily access the most frequent task
        Arrays.sort(taskFreq);
        
        // Identify the frequency of the most frequent task
        int batch = taskFreq[25];

        // Calculate initial idle slots based on the most frequent task frequency
        // We use batch - 1 as we don't need idle time after the last instance of the most frequent task
        int idleSlots = (batch - 1) * n;

        // Try to fill the idle slots with other tasks' occurrences
        for (int i = 0; i < 25; i++) {
            // Deduct the minimum of taskFreq[i] or batch - 1 to avoid overfilling
            idleSlots -= Math.min(taskFreq[i], batch - 1);
        }

        // If there are still idle slots left, add them to the total task count
        // This is because we need to "wait" with idle time between task executions
        if (idleSlots > 0) {
            return idleSlots + tasks.length;
        }

        // If no idle slots are left, return only the total task count
        return tasks.length;
    }
}
```

## 29. [Maximize Happiness of Selected Children](https://leetcode.com/problems/maximize-happiness-of-selected-children/)
###### Explanation Steps:

1. **Initialize Result Variable**:
   - A variable `res` is initialized to zero to accumulate the maximum happiness sum.

2. **Sort the Happiness Array**:
   - The `happiness` array is sorted in ascending order. This allows us to access the highest happiness values easily by starting from the end of the array.

3. **Set Up Counters**:
   - The variable `n` stores the length of the `happiness` array.
   - The `count` variable tracks how many elements have been processed.

4. **Iterate Backwards**:
   - A `for` loop iterates from the last element of the sorted `happiness` array (highest value) to the first element. The loop continues until either all elements have been processed or we have processed `k` elements.

5. **Calculate Effective Happiness**:
   - For each happiness value, we calculate the effective happiness as `happiness[i] - count`. 
   - We use `Math.max(happiness[i] - count, 0)` to ensure that we don’t add negative happiness values to `res`.

6. **Increment Processed Count**:
   - After calculating the happiness for the current element, we increment the `count`.

7. **Return Result**:
   - Finally, the method returns the total accumulated maximum happiness sum (`res`).

```java
class Solution {
    public long maximumHappinessSum(int[] happiness, int k) {
        long res = 0; // Variable to store the total maximum happiness sum
        
        // Sort the happiness array in ascending order
        Arrays.sort(happiness);
        
        int n = happiness.length; // Length of the happiness array
        int count = 0; // Counter to track how many elements we have processed
        
        // Iterate from the end of the sorted happiness array to the beginning
        for (int i = n - 1; i >= 0 && count < k; i--) {
            // Calculate the effective happiness by subtracting the count from the current happiness value
            // Ensure we don't go below zero with Math.max
            res += Math.max(happiness[i] - count, 0);
            
            count++; // Increment the count of processed elements
        }
        
        return res; // Return the total maximum happiness sum
    }
}
```

## 30. [Find the Maximum Sum of Node Values](https://leetcode.com/problems/find-the-maximum-sum-of-node-values/)
###### Explanation of Key Parts
1. **Result Calculation**:
   - `res` accumulates the final sum based on either the original number or its XOR with `k`, depending on which gives a higher result.

2. **Counting Condition Matches**:
   - `count` keeps track of how many times `(num ^ k) > num`, which helps in determining whether `minDiff` needs to be subtracted at the end.
   - When `count` is even, we can use the sum `res` directly. If odd, to meet specific conditions, we reduce the sum slightly by subtracting `minDiff`.

3. **Minimum Difference Tracking**:
   - `minDiff` captures the smallest difference between `num` and `(num ^ k)`. This is useful for minimizing the impact on `res` if an adjustment is necessary to achieve the desired parity for `count`.

This setup provides a solution that maximizes the sum of modified values, taking into account XOR conditions for each `num` in the `nums` array.

```java
class Solution {
    public long maximumValueSum(int[] nums, int k, int[][] edges) {
        // Initialize the result variable to store the maximum possible sum
        long res = 0;
        
        // Count keeps track of how many times (num ^ k) is greater than num
        int count = 0;
        
        // minDiff will store the minimum difference between num and (num ^ k)
        int minDiff = Integer.MAX_VALUE;

        // Iterate through each element in the nums array
        for (int num : nums) {
            // Check if XOR with k gives a larger result than the original num
            if ((num ^ k) > num) {
                // If so, increment the count and add the XOR result to the sum
                count++;
                res += (num ^ k);
            } else {
                // Otherwise, add the original number to the sum
                res += num;
            }
            
            // Update the minimum difference between num and its XOR result with k
            minDiff = Math.min(minDiff, Math.abs(num - (num ^ k)));
        }

        // If count is even, return the result as-is
        // Otherwise, subtract minDiff to make the sum as large as possible while meeting conditions
        if (count % 2 == 0) {
            return res;
        }
        return res - minDiff;
    }
}
```


## 31. [Minimum Increment to Make Array Unique](https://leetcode.com/problems/minimum-increment-to-make-array-unique/)
###### Explanation of Key Steps

1. **Initialize and Populate Frequency Array**: We use `count` as a frequency array where each index represents a possible number in `nums`. Each `count[i]` stores how many times `i` appears in `nums`. The size of `count` is set to `max + n` to account for possible increments pushing values beyond the initial maximum value of `nums`.

2. **Adjust Values to Ensure Uniqueness**: We loop through each index of the `count` array. If the count at a position `i` is greater than 1 (indicating duplicates), we increment values at `i` to make them unique:
   - We calculate the `extra` occurrences (count[i] - 1) that need to be incremented.
   - We add these `extra` occurrences to the next index `count[i + 1]`, effectively "moving" duplicate numbers to higher values.
   - The number of moves required to make values unique increases by the `extra` amount since each increment counts as one move.

3. **Return Result**: The final count of `moves` represents the minimum number of increments required to make all elements in `nums` unique.
```java
class Solution {
    public int minIncrementForUnique(int[] nums) {
        int n = nums.length; // Store the length of the array for easy access
        int max = getMax(nums); // Find the maximum value in the array
        
        // Create a frequency array 'count' to store occurrences of each number.
        // The size is set to max + n to accommodate any incremented values that may go beyond the max.
        int[] count = new int[max + n];
        
        // Populate the count array with the frequency of each number in the input array
        for (int num : nums) {
            count[num]++;
        }
        
        int moves = 0; // Initialize moves counter to track the total increments made

        // Traverse the count array to adjust values and ensure uniqueness
        for (int i = 0; i < count.length - 1; i++) {
            if (count[i] <= 1) {
                // If the count at index i is 0 or 1, no adjustments are needed
                continue;
            }
            
            int extra = count[i] - 1; // Calculate the excess occurrences for value 'i'
            count[i + 1] += extra; // Move excess occurrences to the next value
            moves += extra; // Increase moves by the number of excess occurrences
            
            // This effectively increments values at 'i' to make them unique, 
            // and shifts the extra counts to the next available slots
        }

        // Return the total moves needed to make the array unique
        return moves;
    }

    // Helper method to find the maximum value in the input array
    private int getMax(int[] nums) {
        int mxm = 0; // Initialize maximum with 0 (assuming non-negative numbers in the input)
        for (int x : nums) {
            mxm = Math.max(mxm, x); // Update max if current number x is greater than current max
        }
        return mxm; // Return the maximum value found
    }
}
```

## 32. [Patching Array](https://leetcode.com/problems/patching-array/)
###### Explanation of Key Steps

1. **Initialize Variables**:
   - `maxReach` keeps track of the maximum sum that can be covered with the current numbers in `nums` and any patches added.
   - `i` is the index pointer for iterating through `nums`.
   - `patch` counts the total patches (numbers) added to ensure coverage up to `n`.

2. **Loop Until `maxReach` Covers the Target Range `n`**:
   - The loop continues as long as `maxReach` is less than `n`, meaning we need to keep expanding our coverage.

3. **Expand Coverage Using Elements in `nums` or Adding a Patch**:
   - **Condition 1**: If the current number `nums[i]` is within the next reachable range (`nums[i] <= maxReach + 1`), we add it to `maxReach` and move to the next number (`i++`). This incrementally expands the range we can reach.
   - **Condition 2**: If `nums[i]` is too large to fill the next gap or we've exhausted `nums`, we add a patch with the smallest missing number, `maxReach + 1`, to fill the gap. This effectively increases `maxReach` and expands the range we can cover.

4. **Return Result**:
   - Once `maxReach` covers the range `[1, n]`, the loop exits, and the `patch` counter (total patches added) is returned. This count represents the minimum number of additional numbers required to cover all numbers from `1` to `n`.

```java
class Solution {
    public int minPatches(int[] nums, int n) {
        long maxReach = 0; // Represents the maximum sum that can be reached with current numbers and patches
        int i = 0; // Pointer for iterating through the nums array
        int patch = 0; // Counter for the number of patches (numbers added) required
        
        // Continue until we can cover the entire range from 1 to n
        while (maxReach < n) {
            // If the current number in nums is within the range we can reach (maxReach + 1)
            if (i < nums.length && nums[i] <= maxReach + 1) {
                maxReach += nums[i]; // Extend maxReach by adding the current number
                i++; // Move to the next number in nums
            } else {
                // If nums[i] is too large or we've used all elements in nums,
                // add a new patch (maxReach + 1) to extend the reachable range
                maxReach += (maxReach + 1); // Add the smallest missing number to reach the next gap
                patch++; // Increment patch count since we added a new number
            }
        }
        
        // Return the total number of patches needed to cover range 1 to n
        return patch;
    }
}
```

## 33. [Minimum Cost for Cutting Cake I](https://leetcode.com/problems/minimum-cost-for-cutting-cake-i/)
###### Explanation of Key Steps

1. **Initialize and Sort Cuts**:
   - The `horizontalCut` and `verticalCut` arrays are sorted in ascending order so that we can process them from largest to smallest by iterating from the end of the arrays.

2. **Tracking Pieces**:
   - `horPieces` and `verPieces` start at 1, representing the initial full piece. As cuts are applied, these counts are increased to track how many sections have been created in each direction.

3. **Calculate the Total Cost with Maximum Cuts First**:
   - By prioritizing larger cuts first (using a descending order traversal), we ensure that larger cuts are applied to the largest number of pieces, maximizing cost efficiency.
   - **Choosing Cuts**: In each iteration, compare the largest remaining `horizontalCut` and `verticalCut`.
     - If the current `horizontalCut` is larger, apply it to all vertical sections, update `totalCost`, increase `horPieces`, and move to the next horizontal cut.
     - If the current `verticalCut` is larger, apply it to all horizontal sections, update `totalCost`, increase `verPieces`, and move to the next vertical cut.

4. **Process Remaining Cuts**:
   - After either `horizontalCut` or `verticalCut` has been exhausted, any remaining cuts in the other array are applied to all current pieces in the opposite direction.

5. **Return Result**:
   - The result, `totalCost`, is returned as an integer after casting. This represents the minimum cost needed to split the grid with the provided cuts.

```java
import java.util.Arrays;

class Solution {
    public int minimumCost(int m, int n, int[] horizontalCut, int[] verticalCut) {
        // Sort the horizontal and vertical cuts in descending order to maximize the cost at each step
        Arrays.sort(horizontalCut);
        Arrays.sort(verticalCut);

        int horPieces = 1; // Start with one horizontal piece
        int verPieces = 1; // Start with one vertical piece

        long totalCost = 0; // Initialize total cost to accumulate the minimum cost

        int i = horizontalCut.length - 1; // Pointer for horizontal cuts, starting from the largest cut
        int j = verticalCut.length - 1;   // Pointer for vertical cuts, starting from the largest cut

        // Process both arrays of cuts until all cuts are added to maximize cost at each step
        while (i >= 0 && j >= 0) {
            if (horizontalCut[i] >= verticalCut[j]) {
                // If the horizontal cut is larger, use it to make `verPieces` vertical sections
                totalCost += (long) horizontalCut[i] * verPieces;
                horPieces++; // Increase horizontal pieces count as this cut is used
                i--; // Move to the next largest horizontal cut
            } else {
                // If the vertical cut is larger, use it to make `horPieces` horizontal sections
                totalCost += (long) verticalCut[j] * horPieces;
                verPieces++; // Increase vertical pieces count as this cut is used
                j--; // Move to the next largest vertical cut
            }
        }

        // If there are any remaining vertical cuts after finishing horizontal cuts
        while (j >= 0) {
            totalCost += (long) verticalCut[j] * horPieces;
            verPieces++;
            j--;
        }

        // If there are any remaining horizontal cuts after finishing vertical cuts
        while (i >= 0) {
            totalCost += (long) horizontalCut[i] * verPieces;
            horPieces++;
            i--;
        }

        // Convert the result to an integer and return it, as required
        return (int) totalCost;
    }
}
```

## 34. [Minimum Cost for Cutting Cake II](https://leetcode.com/problems/minimum-cost-for-cutting-cake-ii/)
###### Explanation of Key Steps

1. **Sorting Cuts**:
   - `horizontalCut` and `verticalCut` arrays are sorted in ascending order, so we can iterate from the largest cuts by starting at the end of the arrays.

2. **Tracking Number of Pieces**:
   - `horPieces` and `verPieces` start at 1, representing a single, uncut piece in each dimension. As cuts are applied, these counts are incremented to reflect how many sections exist in each direction.

3. **Calculate Total Cost by Prioritizing Largest Cuts**:
   - By using a greedy approach, we process the largest remaining cuts first. This ensures that the largest cuts are applied to the maximum number of sections, optimizing the total cost.
   - In each iteration:
     - **If the largest horizontal cut is greater than or equal to the largest vertical cut**, apply it to all vertical pieces, add the corresponding cost to `totalCost`, increment `horPieces`, and move to the next horizontal cut.
     - **Otherwise**, apply the largest vertical cut to all horizontal pieces, add the cost, increment `verPieces`, and move to the next vertical cut.

4. **Handling Remaining Cuts**:
   - If there are remaining cuts in `verticalCut` after `horizontalCut` is exhausted, or vice versa, each remaining cut is applied to all current pieces in the other direction.

5. **Return the Result**:
   - The final `totalCost` is returned, representing the minimum cost to split the grid according to the given cuts.

```java
import java.util.Arrays;

class Solution {
    public long minimumCost(int m, int n, int[] horizontalCut, int[] verticalCut) {
        // Sort horizontal and vertical cuts in ascending order
        Arrays.sort(horizontalCut);
        Arrays.sort(verticalCut);

        int horPieces = 1; // Initial number of horizontal pieces
        int verPieces = 1; // Initial number of vertical pieces

        long totalCost = 0; // Variable to store the total cost

        int i = horizontalCut.length - 1; // Start from the largest horizontal cut
        int j = verticalCut.length - 1;   // Start from the largest vertical cut

        // Process both cuts arrays until all cuts are accounted for
        while (i >= 0 && j >= 0) {
            // If the largest remaining horizontal cut is greater than or equal to the largest vertical cut
            if (horizontalCut[i] >= verticalCut[j]) {
                // Use the horizontal cut and apply it across all current vertical pieces
                totalCost += (long) horizontalCut[i] * verPieces;
                horPieces++; // Increase the count of horizontal pieces
                i--; // Move to the next horizontal cut
            } else {
                // Use the vertical cut and apply it across all current horizontal pieces
                totalCost += (long) verticalCut[j] * horPieces;
                verPieces++; // Increase the count of vertical pieces
                j--; // Move to the next vertical cut
            }
        }

        // Process any remaining vertical cuts
        while (j >= 0) {
            // Apply remaining vertical cuts across all horizontal pieces
            totalCost += (long) verticalCut[j] * horPieces;
            verPieces++;
            j--;
        }

        // Process any remaining horizontal cuts
        while (i >= 0) {
            // Apply remaining horizontal cuts across all vertical pieces
            totalCost += (long) horizontalCut[i] * verPieces;
            horPieces++;
            i--;
        }

        // Return the total calculated cost
        return totalCost;
    }
}
```

## 35. [Maximum Distance in Arrays](https://leetcode.com/problems/maximum-distance-in-arrays/)
###### Explanation
1. **Initialize Variables**:
   - `res` is initialized to 0. It will store the maximum distance found.
   - `min` is set to the first element of the first list in `arrays`, representing the minimum element so far.
   - `max` is set to the last element of the first list in `arrays`, representing the maximum element so far.

2. **Iterate Through the Remaining Lists**:
   - A loop starts from the second list (`i = 1`) and iterates through each list in `arrays`.
   - For each list (`cur`), `curMin` is set to the first element (smallest in the list) and `curMax` to the last element (largest in the list).

3. **Calculate Maximum Distance**:
   - The code calculates two possible distances for each list:
     - The distance between `curMax` (largest element of the current list) and `min` (smallest element encountered so far).
     - The distance between `curMin` (smallest element of the current list) and `max` (largest element encountered so far).
   - `res` is updated with the larger of its current value or the maximum of these two distances.

4. **Update Global Min and Max**:
   - `max` is updated to the maximum of its current value and `curMax`, tracking the largest element encountered so far.
   - `min` is updated to the minimum of its current value and `curMin`, tracking the smallest element encountered so far.

5. **Return the Result**:
   - After all lists have been processed, `res` contains the maximum distance, and this value is returned as the result. 

This approach ensures that we consider only the first and last elements in each list, which are the smallest and largest for that list. By comparing each list’s boundaries (min and max) with previously recorded global boundaries, we efficiently calculate the maximum possible distance without needing to check each element in each list.

```java
class Solution {
    public int maxDistance(List<List<Integer>> arrays) {
        int res = 0;
        int min = arrays.get(0).get(0);
        int max = arrays.get(0).get(arrays.get(0).size()-1);

        for (int i=1; i<arrays.size(); i++){
            List<Integer> cur = arrays.get(i);
            int curMin = cur.get(0);
            int curMax = cur.get(cur.size()-1);
            
            res = Math.max(res, Math.max(Math.abs(curMax-min),Math.abs(curMin-max)));

            max = Math.max(max,curMax);
            min = Math.min(min,curMin);
        }
        return res;
    }
}
```