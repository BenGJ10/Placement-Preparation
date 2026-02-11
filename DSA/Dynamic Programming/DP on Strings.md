# DP on Strings
Dynamic Programming on Strings is one of the most important interview topics because many problems involve:

* matching sequences
* editing strings
* counting transformations
* finding subsequences
* palindrome-based logic

Most string DP problems revolve around comparing **two indices**.

---

## When to Recognize DP on Strings

Look for keywords like:

* subsequence
* substring
* palindrome
* insert/delete/replace
* transformations
* matching
* minimum operations
* longest common

If recursion compares characters from two strings, DP is almost guaranteed.

---

## Core DP State

Most string DP problems use:

```
dp[i][j]
```

Meaning:

> Answer using first `i` characters of string1 and first `j` characters of string2.

Always define the state **in words** before coding.

---

## Pattern 1: Longest Common Subsequence (LCS)

This is the **foundation** of string DP.

Many problems reduce to LCS.

---

### Problem

**Find the longest subsequence present in both strings**.

Example:

```
s1 = "abcde"
s2 = "ace"

LCS = "ace"
length = 3
```

---

### Recurrence

If characters match:

```
dp[i][j] = 1 + dp[i-1][j-1]
```

Else:

```
dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

---

### Tabulation

```cpp
int LCS(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n+1, vector<int>(m+1, 0));

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (a[i-1] == b[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[n][m];
}
```

---

### Space Optimization

Only previous row needed.

```cpp
int LCS(string a, string b) {
    int n = a.size(), m = b.size();
    vector<int> prev(m+1, 0), cur(m+1, 0);

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (a[i-1] == b[j-1])
                cur[j] = 1 + prev[j-1];
            else
                cur[j] = max(prev[j], cur[j-1]);
        }
        prev = cur;
    }
    return prev[m];
}
```

---

## Pattern 2: Longest Common Substring

Unlike subsequence:

* must be contiguous
* reset when mismatch occurs

---

### Recurrence

```
if match:
    dp[i][j] = 1 + dp[i-1][j-1]
else:
    dp[i][j] = 0
```

Track global maximum.

---

### Code

```cpp
int longestCommonSubstring(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n+1, vector<int>(m+1, 0));

    int ans = 0;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (a[i-1] == b[j-1]) {
                dp[i][j] = 1 + dp[i-1][j-1];
                ans = max(ans, dp[i][j]);
            }
        }
    }
    return ans;
}
```

---

## Pattern 3: Edit Distance (Levenshtein Distance)

Find minimum operations to convert one string to another.

Allowed operations:

* insert
* delete
* replace

---

### Recurrence

If characters match:

```
dp[i][j] = dp[i-1][j-1]
```

Else:

```
1 + min(
    dp[i-1][j],    // delete
    dp[i][j-1],    // insert
    dp[i-1][j-1]   // replace
)
```

---

### Code

```cpp
int editDistance(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n+1, vector<int>(m+1));

    for (int i = 0; i <= n; i++)
        dp[i][0] = i;

    for (int j = 0; j <= m; j++)
        dp[0][j] = j;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (a[i-1] == b[j-1])
                dp[i][j] = dp[i-1][j-1];
            else
                dp[i][j] = 1 + min({
                    dp[i-1][j],
                    dp[i][j-1],
                    dp[i-1][j-1]
                });
        }
    }
    return dp[n][m];
}
```

---

## Pattern 4: Shortest Common Supersequence

Smallest string containing both as subsequences.

Key relation:

```
len = n + m - LCS
```

To print the string, backtrack through LCS table.

---

## Pattern 5: Longest Palindromic Subsequence

Trick:

Reverse the string and compute LCS.

```
LPS(s) = LCS(s, reverse(s))
```

---

## Pattern 6: Minimum Insertions to Make Palindrome

Another LCS transformation:

```
answer = n - LPS
```

---

## Pattern 7: Distinct Subsequences

Count ways string `s` forms `t`.

### Recurrence

If characters match:

```
take + skip
```

Else:

```
skip
```

---

### Code

```cpp
long long numDistinct(string s, string t) {
    int n = s.size(), m = t.size();
    vector<vector<long long>> dp(n+1, vector<long long>(m+1, 0));

    for (int i = 0; i <= n; i++)
        dp[i][0] = 1;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (s[i-1] == t[j-1])
                dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
            else
                dp[i][j] = dp[i-1][j];
        }
    }
    return dp[n][m];
}
```

---

## Common DP on Strings Patterns Summary

| Pattern               | Base Idea             |
| --------------------- | --------------------- |
| LCS                   | match vs skip         |
| substring             | reset on mismatch     |
| edit distance         | insert/delete/replace |
| palindrome            | reverse + LCS         |
| supersequence         | LCS relation          |
| counting subsequences | take / not take       |

---

## Typical Complexity

Most string DP:

```
Time: O(n × m)
Space: O(n × m)
Optimized: O(min(n,m))
```

---

## Common Mistakes

* confusing substring vs subsequence
* incorrect dp base cases
* off-by-one indexing
* not shifting indices (dp size n+1)
* forgetting space optimization
* using recursion → TLE

---

## Interview Questions (With Answers)

### Q1. Why is LCS foundational?

Because many problems reduce to it through transformations.

---

### Q2. How to differentiate substring vs subsequence DP?

Substring resets on mismatch.
Subsequence takes max of neighbors.

---

### Q3. Why dp is sized (n+1)(m+1)?

To handle empty prefixes cleanly.

---

### Q4. Can string DP be optimized to 1D?

Yes, when transitions depend only on previous row.

---

### Q5. Why is edit distance harder than LCS?

Because it has three transitions instead of two.

---

## LeetCode Problems (DP on Strings)

* [https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)
* [https://leetcode.com/problems/edit-distance/](https://leetcode.com/problems/edit-distance/)
* [https://leetcode.com/problems/distinct-subsequences/](https://leetcode.com/problems/distinct-subsequences/)
* [https://leetcode.com/problems/longest-palindromic-subsequence/](https://leetcode.com/problems/longest-palindromic-subsequence/)
* [https://leetcode.com/problems/shortest-common-supersequence/](https://leetcode.com/problems/shortest-common-supersequence/)
* [https://leetcode.com/problems/delete-operation-for-two-strings/](https://leetcode.com/problems/delete-operation-for-two-strings/)
* [https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)
* [https://leetcode.com/problems/is-subsequence/](https://leetcode.com/problems/is-subsequence/)

---

## How to Master String DP

Follow this order:

1. LCS
2. Longest Common Substring
3. Edit Distance
4. Palindromic Subsequence
5. Distinct Subsequences
6. Supersequence

Most interview questions derive from these.

---

## Summary

* DP on strings compares prefixes using `dp[i][j]`

* LCS is the most important base pattern

* Many problems are transformations of LCS

* Complexity usually O(n × m)

* Correct state definition is the hardest part

---
