

## [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)


```java
class Solution {
    List<List<String>> res;
    String str;
    public List<List<String>> partition(String s) {
        str = s;
        res = new ArrayList<>();
        dfs(0,new ArrayList<>());
        return res;
    }

    private void dfs(int ind, List<String> sub)
    {
        // base case
        if (ind == str.length())
        {
            res.add(new ArrayList<>(sub));
            return;
        }

        for (int i = ind; i < str.length(); i++)
        {
            String subStr = str.substring(ind,i+1);
            if (isPalindrome(subStr))
            {
                sub.add(subStr);
                dfs(i+1, sub);
                sub.remove(sub.size()-1);
            }
        }
    }


    private boolean isPalindrome(String s)
    {
        int i = 0;
        int j = s.length()-1;

        while (i < j)
        {
            if (s.charAt(i)!= s.charAt(j))
            {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```