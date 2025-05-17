# [Word Break II](https://www.geeksforgeeks.org/problems/word-break-part-23249/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Backtracking | Google, Amazon      | [Video]()   |
|            |              | Microsoft, Facebook | [Article](https://www.geeksforgeeks.org/word-break-problem-using-backtracking/) |

## Description
Given a non-empty string s and a dictionary dict[] containing a list of non-empty words, the task is to return all possible ways to break the string in individual dictionary words.
Note: The same word in the dictionary may be reused multiple times while breaking.

**Examples:** 
```
Example 1:
Input: s = “likegfg” , dict[] = [“lik”, “like”, “egfg”, “gfg”]
Output: 
“lik egfg” 
“like gfg”
Explanation: The string "likegfg" is segmented into valid dictionary words in all possible ways: "lik egfg" and "like gfg".


Example 2:
Input: s = “geeksforgeeks” , dict[] = [“for”, “geeks”]
Output: 
“geeks for geeks” 
Explanation: The string “geeksforgeeks” can be broken into valid words from the dictionary in one ways
```

## Using Recursion – O((2^n) * k) Time and O(n) Space
- Include the current substring in the solution, If the substring exists in the dictionary, recursively check for the rest of the string starting from the next index.

- Skip the current substring and move to the next possible substring starting from the same index.
    `wordBreak(s,start) = wordBreak(s, end) , if s[start:end] ∈ dictionary`

**Base Case** : wordBreak(s, start) = true, this signifies that a valid sentence has been constructed for the given input string.

### Algorithm
1. Convert the dictionary into a hash set for quick search.
2. If the start index reaches the length of the string (s), it signifies a valid sentence has been constructed. Add the current sentence (curr) to the result.
3. Loop through every substring starting at start and ending at all possible positions (end).
4. For each substring, check if it exists in the dictionary (dictSet).
5. If valid:
    - Append the word to the current sentence (curr).
    - Recursively call the function for the remaining part of the string (from end onwards).
6. After the recursive call returns, restore the state of curr to ensure the next branch of exploration starts with the correct sentence.

### Code
```java
class Solution {

    // Helper function to perform backtracking
    static void wordBreakHelper(String s, HashSet<String> dictSet, 
                                       String curr, List<String> res, 
                                       int start) {

        // If start reaches the end of the string,
        // save the result
        if (start == s.length()) {
            res.add(curr);
            return;
        }

        // Try every possible substring from the current index
        for (int end = start + 1; end <= s.length(); ++end) {
            String word = s.substring(start, end);

            // Check if the word exists in the dictionary
            if (dictSet.contains(word)) {
                String prev = curr;

                // Append the word to the current sentence
                if (!curr.isEmpty()) {
                    curr += " ";
                }
                curr += word;

                // Recurse for the remaining string
                wordBreakHelper(s, dictSet, curr, res, end);

                // Backtrack to restore the current sentence
                curr = prev;
            }
        }
    }

    // Main function to generate all possible sentence breaks
    static List<String> wordBreak(String s, List<String> dict) {

        // Convert dictionary vector to a HashSet
        HashSet<String> dictSet = new HashSet<>(dict);

        List<String> res = new ArrayList<>();
        String curr = "";

        wordBreakHelper(s, dictSet, curr, res, 0);

        return res;
    }
}
```

### Complexity Analysis

- **Time Complexity**: O((2^n) * k), for a string of length n, there are 2^n possible partitions, and each substring check takes O(k) time (average substring length k), leading to O((2^n) * k).

- **Auxiliary Space**: O(n), due to the recursion stack can go as deep as O(n) in the worst case.



## Using Top-Down DP (Memoization) – O((n^2)*m) Time and O(n*m) Space

```
1. Optimal Substructure: The solution to the word break problem can be derived from the solutions of smaller subproblems. Specifically, for any given starting index start, the problem reduces to determining if the substring s[start..end] forms a valid word and combining this with valid sentence breaks for the remaining substring s[end..n].


We can express the recursive relation as follows:


For every substring s[start..end] where end > start, check: If the word exists in the dictionary st.count(s.substr(start, end – start)).
For each valid result of wordBreakHelper(s, st, memo, end):
If it is empty, add only the word.
Otherwise, concatenate the word with the sub-sentence using a space.

2. Overlapping Subproblems: When solving the word break problem using recursion, the same subproblems are solved multiple times. For example, if we compute wordBreakHelper(s, st, memo, start) for a certain start, it will recompute results for the same start index in different branches of recursion. To avoid recomputation, we store the results of wordBreakHelper(s, st, memo, start) in a memo map where the key is the index start, and the value is a list of valid sentence breaks starting from that index.
```

### Code
```java
import java.util.*;

class Solution {
    // Helper function to perform backtracking with
    // memoization
    static String[] wordBreakHelper(
        String s, HashSet<String> st,
        Map<Integer, String[]> memo, int start)
    {

        // If the result for this index is already computed,
        // return it
        if (memo.containsKey(start)) {
            return memo.get(start);
        }

        ArrayList<String> resultList = new ArrayList<>();

        // If start reaches the end of the string,
        // add an empty string to signify a valid sentence
        if (start == s.length()) {
            return new String[] { "" };
        }

        // Try every possible substring from the current
        // index
        for (int end = start + 1; end <= s.length();
             ++end) {
            String word = s.substring(start, end);

            // Check if the word exists in the set
            if (st.contains(word)) {
                // Recurse for the remaining string
                String[] subSentences
                    = wordBreakHelper(s, st, memo, end);

                // Append the current word to each valid
                // sub-sentence
                for (String sub : subSentences) {
                    if (sub.isEmpty()) {
                        resultList.add(word);
                    }
                    else {
                        resultList.add(word + " " + sub);
                    }
                }
            }
        }

        // Convert ArrayList to Array and Memoize the result
        // for the current index
        String[] resultArray
            = resultList.toArray(new String[0]);
        memo.put(start, resultArray);
        return resultArray;
    }

    // Main function to generate all possible sentence
    // breaks
    static String[] wordBreak(String[] dict, String s)
    {

        // Convert words array to a HashSet
        HashSet<String> st
            = new HashSet<>(Arrays.asList(dict));

        Map<Integer, String[]> memo = new HashMap<>();

        return wordBreakHelper(s, st, memo, 0);
    }

    public static void main(String[] args)
    {

        String s = "likegfg";
        String[] dict = {"lik", "like", "egfg", "gfg"  };

        String[] result = wordBreak(dict, s);

        for (String sentence : result) {
            System.out.println(sentence);
        }
    }
}
```

### Complexity Analysis

- **Time Complexity** : O((n^2)*m) 
- **Space** : O(n*m)