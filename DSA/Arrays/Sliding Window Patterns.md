# Sliding Window – Important Patterns

**Sliding Window** is a technique used to solve problems involving **contiguous subarrays or substrings** efficiently.

Instead of recomputing results for every window from scratch, we:

* maintain a **current window**
* expand or shrink it using pointers
* update the answer incrementally

This transforms **O(n²)** brute-force solutions into **O(n)** or **O(n log n)**.

Sliding window is commonly used in:

* longest/shortest substring problems
* subarrays with sum constraints
* frequency/count problems
* streaming data processing

---

## Types of Sliding Windows

Two primary types:

1. **Fixed-size window**
2. **Variable-size window**

   * based on conditions (sum, distinct chars, etc.)

We also discuss:

* at most K condition windows
* exactly K condition windows

---

## 1) Fixed-Size Sliding Window

Window size `k` is **known in advance**.

### Pattern

* expand right pointer
* shrink left pointer when size exceeds k
* maintain running sum/frequency/etc.

### Template

```cpp
int l = 0;
for (int r = 0; r < n; r++) {
    // add a[r] to window

    if (r - l + 1 > k) {
        // remove a[l]
        l++;
    }

    if (r - l + 1 == k) {
        // compute/store answer
    }
}
```

---

### Example: Maximum Sum Subarray of Size K

```cpp
int maxSumK(vector<int>& a, int k) {
    int n = a.size();
    int sum = 0, ans = INT_MIN;

    int l = 0;
    for (int r = 0; r < n; r++) {
        sum += a[r];

        if (r - l + 1 > k) {
            sum -= a[l];
            l++;
        }

        if (r - l + 1 == k) {
            ans = max(ans, sum);
        }
    }
    return ans;
}
```

---

## 2) Variable-Size Sliding Window

Window size is **not fixed**.
We expand and shrink based on condition.

Typical conditions:

* sum ≤ K
* at most K distinct characters
* frequency constraint
* product constraint

---

### General Template – Grow and Shrink

```cpp
int l = 0;

for (int r = 0; r < n; r++) {
    // expand by including a[r]

    while (condition is violated) {
        // shrink from left
        l++;
    }

    // answer update
}
```

Key: maintain invariants like sum, count, map, etc.

---

## 3) Longest Substring with At Most K Distinct Characters

This is a **variable window with map**.

```cpp
int lengthOfLongestSubstringKDistinct(string s, int k) {
    unordered_map<char,int> mp;
    int l = 0, ans = 0;

    for (int r = 0; r < s.size(); r++) {
        mp[s[r]]++;

        while (mp.size() > k) {
            mp[s[l]]--;
            if (mp[s[l]] == 0) mp.erase(s[l]);
            l++;
        }

        ans = max(ans, r - l + 1);
    }
    return ans;
}
```

---

## 4) Longest Substring Without Repeating Characters

Special case of previous (k = all unique characters allowed).

```cpp
int lengthOfLongestSubstring(string s) {
    vector<int> last(256, -1);
    int l = 0, ans = 0;

    for (int r = 0; r < s.size(); r++) {
        if (last[s[r]] >= l)
            l = last[s[r]] + 1;

        last[s[r]] = r;
        ans = max(ans, r - l + 1);
    }
    return ans;
}
```

---

## 5) Number of Subarrays with Sum ≤ K (At Most K Condition)

This pattern is extremely powerful.

### Key trick

If you can count subarrays with constraint **at most K**, then:

```
exactly K = atMost(K) − atMost(K−1)
```

---

### Example: Subarrays with Product Less Than K

```cpp
int numSubarrayProductLessThanK(vector<int>& a, int k) {
    if (k <= 1) return 0;

    int prod = 1, l = 0, ans = 0;

    for (int r = 0; r < a.size(); r++) {
        prod *= a[r];

        while (prod >= k) {
            prod /= a[l];
            l++;
        }

        ans += r - l + 1;
    }
    return ans;
}
```

---

## Numbers of Subarrays with Sum Exactly K

In this we we use the atMost trick. We find number of subarrays with sum at most K and subtract number of subarrays with sum at most K-1.

```cpp
int atMostK(vector<int>& a, int k) {
    int sum = 0, l = 0, ans = 0;

    for (int r = 0; r < a.size(); r++) {
        sum += a[r];

        while (sum > k) {
            sum -= a[l];
            l++;
        }

        ans += r - l + 1;
    }
    return ans;
}

int subarraySumExactlyK(vector<int>& a, int k) {
    return atMostK(a, k) - atMostK(a, k - 1);
}
```

---

## 7) Fixed vs Variable Window Summary

| Feature  | Fixed Window          | Variable Window                  |
| -------- | --------------------- | -------------------------------- |
| Size     | Known                 | Changes                          |
| Logic    | add/remove fixed size | condition-driven                 |
| Examples | max sum size K        | longest substring without repeat |

---

## 8) Sliding Window with Deque (Monotonic Window)

Used for:

* max/min in each subarray of size k
* maintain decreasing deque

---

### Example: Sliding Window Maximum

```cpp
vector<int> maxSlidingWindow(vector<int>& a, int k) {
    deque<int> dq;
    vector<int> ans;

    for (int i = 0; i < a.size(); i++) {

        while (!dq.empty() && dq.front() <= i - k)
            dq.pop_front();

        while (!dq.empty() && a[dq.back()] <= a[i])
            dq.pop_back();

        dq.push_back(i);

        if (i >= k - 1)
            ans.push_back(a[dq.front()]);
    }
    return ans;
}
```

---

## 9) Pattern: Expand–Contract Until Condition Met

Typical for:

* minimum window length problems
* shortest subarray covering conditions

General flow:

1. expand right to satisfy condition
2. shrink left to optimize size

---

## Recognizing Sliding Window Questions

Look for keywords:

* subarray / substring
* longest / shortest
* at most / exactly K
* contiguous
* window / streaming data
* sum / product constraint

When brute force is O(n²), sliding window usually reduces to O(n).

---

## Common Mistakes

* mixing fixed and variable logic
* forgetting to shrink window
* using wrong condition direction
* not maintaining frequency counts properly
* integer overflow on product windows
* ignoring negative numbers (simple sum≤k trick fails)

---

## Interview Questions (With Answers)

### Q1. When does sliding window fail?

When window elements are **not contiguous**, or answer depends on global properties rather than local window.

---

### Q2. Can sliding window be used if numbers are negative?

Yes, but **sum-based variable window logic breaks**; extra structures like prefix sums or maps are needed.

---

### Q3. Difference between Sliding Window and Two Pointers?

Sliding window always refers to **contiguous** segment.
Two pointers technique is more general.

---

### Q4. Why is deque needed in max sliding window?

To efficiently maintain maximum in **O(1)** amortized time per element.

---

### Q5. Why does atMost(K) trick work?

Because counting subarrays with at most K condition is **monotonic** (adding constraints shrinks set).

---

## LeetCode Problems to Practice Sliding Window Patterns

* [https://leetcode.com/problems/maximum-average-subarray-i/](https://leetcode.com/problems/maximum-average-subarray-i/)
* [https://leetcode.com/problems/maximum-subarray-of-size-k/](https://leetcode.com/problems/maximum-subarray-of-size-k/)
* [https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
* [https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
* [https://leetcode.com/problems/minimum-window-substring/](https://leetcode.com/problems/minimum-window-substring/)
* [https://leetcode.com/problems/fruit-into-baskets/](https://leetcode.com/problems/fruit-into-baskets/)
* [https://leetcode.com/problems/permutation-in-string/](https://leetcode.com/problems/permutation-in-string/)
* [https://leetcode.com/problems/subarrays-with-k-different-integers/](https://leetcode.com/problems/subarrays-with-k-different-integers/)
* [https://leetcode.com/problems/number-of-subarrays-with-bounded-maximum/](https://leetcode.com/problems/number-of-subarrays-with-bounded-maximum/)
* [https://leetcode.com/problems/sliding-window-maximum/](https://leetcode.com/problems/sliding-window-maximum/)

---

## Summary

* Sliding window reduces O(n²) contiguous subarray problems to O(n)
* Two major categories:

  * fixed window
  * variable window
* Patterns:

  * longest/shortest window
  * at most / exactly K condition
  * frequency/hashmap window
  * product/sum constrained
  * monotonic deque optimization
* Know templates and adjust for constraints

---
