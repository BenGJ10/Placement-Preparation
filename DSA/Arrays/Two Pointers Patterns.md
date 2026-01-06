# Two Pointers – Important Patterns

**Two Pointers** is a fundamental problem-solving technique used when working with:

* arrays
* strings
* linked lists

The key idea is to maintain **two indices** that move through the structure in specific ways to achieve optimal time complexity, usually **O(n)**.

It is extremely common in:

* searching pairs/triplets
* removing elements in-place
* sliding windows
* palindrome/string problems
* sorted array processing
* merging sorted structures

---

## Why Two Pointers Work

Two pointers exploit one or more of these facts:

* array/string has **order** (sorted or positional)
* we can **avoid nested loops**
* many problems depend only on **relative positions**, not values alone

Instead of brute-force O(n²), two pointers often reduce solution to **O(n)**.

---

## Core Two Pointer Patterns

We now cover main patterns:

1. Opposite direction pointers (left–right)
2. Same direction pointers (fast–slow)
3. Two pointers on sorted array
4. Two pointers + sorting
5. Removing / compressing in-place
6. String two-pointer patterns
7. Linked list two-pointer patterns
8. Two pointers with sliding window

---

## 1) Opposite Direction Pointers (Left and Right)

### When to use

* array is sorted or can be sorted
* need pair/triplet sum / difference
* need to shrink search space from both ends

### Typical structure

```cpp
int l = 0, r = n - 1;
while (l < r) {
    // logic using a[l], a[r]
}
```

---

### Example: Two Sum in Sorted Array

Find two numbers whose sum equals target.

```cpp
vector<int> twoSumSorted(vector<int>& a, int target) {
    int l = 0, r = a.size() - 1;

    while (l < r) {
        int sum = a[l] + a[r];
        if (sum == target) return {l, r};
        else if (sum < target) l++;
        else r--;
    }
    return {-1, -1};
}
```

---

## 2) Same Direction Pointers (Fast & Slow)

### When to use

* detect cycles
* remove duplicates
* find middle of linked list
* in-place transformations

---

### Example: Remove Duplicates from Sorted Array

```cpp
int removeDuplicates(vector<int>& a) {
    int n = a.size();
    if (n == 0) return 0;

    int slow = 0;

    for (int fast = 1; fast < n; fast++) {
        if (a[fast] != a[slow]) {
            slow++;
            a[slow] = a[fast];
        }
    }
    return slow + 1;
}
```

Idea:
`fast` explores, `slow` builds result in-place.

---

## 3) Fast–Slow Pointers in Linked Lists

### Key applications

* detect cycle (Floyd’s algorithm)
* find cycle start
* find middle node

---

### Example: Detect Cycle in Linked List

```cpp
bool hasCycle(ListNode* head) {
    ListNode *slow = head, *fast = head;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;

        if (slow == fast) return true;
    }
    return false;
}
```

---

## 4) Two Pointers After Sorting (Value-Based)

Array not sorted initially → **sort**, then apply pointers.

Useful for:

* triplet sum
* 3Sum / 4Sum style problems
* minimizing difference pairs

---

## Example: 3Sum

```cpp
vector<vector<int>> threeSum(vector<int>& a) {
    sort(a.begin(), a.end());
    vector<vector<int>> res;

    for (int i = 0; i < a.size(); i++) {
        if (i > 0 && a[i] == a[i-1]) continue;

        int l = i + 1, r = a.size() - 1;

        while (l < r) {
            int sum = a[i] + a[l] + a[r];

            if (sum == 0) {
                res.push_back({a[i], a[l], a[r]});
                while (l < r && a[l] == a[l+1]) l++;
                while (l < r && a[r] == a[r-1]) r--;
                l++; r--;
            }
            else if (sum < 0) l++;
            else r--;
        }
    }
    return res;
}
```

---

# 5) Partitioning / Segregation Problems

Two pointers used to **rearrange** elements in-place.

### Examples

* segregate 0s and 1s
* move negatives left positives right
* Dutch national flag (0,1,2) — 3-way partitioning

---

## Example: Move Zeroes to End

```cpp
void moveZeroes(vector<int>& a) {
    int slow = 0;

    for (int fast = 0; fast < a.size(); fast++) {
        if (a[fast] != 0) {
            swap(a[slow], a[fast]);
            slow++;
        }
    }
}
```

---

# 6) Two Pointers in Strings

### Use cases

* palindrome checking
* valid substring
* skip characters conditions
* removing characters

---

## Example: Valid Palindrome

```cpp
bool isPalindrome(string s) {
    int l = 0, r = s.size() - 1;

    while (l < r) {
        while (l < r && !isalnum(s[l])) l++;
        while (l < r && !isalnum(s[r])) r--;

        if (tolower(s[l]) != tolower(s[r])) return false;

        l++; r--;
    }
    return true;
}
```

---

# 7) Sliding Window + Two Pointers

Two pointers moving forward while maintaining a **window**.

Used for:

* longest substring without repeating characters
* longest subarray with condition
* variable window pattern

---

## Example: Longest Substring Without Repeating Characters

```cpp
int lengthOfLongestSubstring(string s) {
    vector<int> freq(256, -1);

    int l = 0, ans = 0;

    for (int r = 0; r < s.size(); r++) {
        if (freq[s[r]] >= l)
            l = freq[s[r]] + 1;

        freq[s[r]] = r;
        ans = max(ans, r - l + 1);
    }
    return ans;
}
```

---

# 8) Meeting in the Middle Variant

Works with sorted arrays and target comparison conditions.

Useful for:

* minimize difference
* closest sum
* container with most water

---

## Example: Container With Most Water

```cpp
int maxArea(vector<int>& h) {
    int l = 0, r = h.size() - 1, ans = 0;

    while (l < r) {
        ans = max(ans, min(h[l], h[r]) * (r - l));
        if (h[l] < h[r]) l++;
        else r--;
    }
    return ans;
}
```

---

# Common Mistakes

* forgetting array must be sorted in some problems
* moving wrong pointer direction
* infinite loops on equal case
* not handling duplicates
* confusing sliding window vs two pointers
* off-by-one errors

---

# How to Recognize Two-Pointer Problems

Look for:

* sorted array + target
* pair/triplet questions
* “in-place”
* O(1) space requirement
* avoid nested loops
* substrings/subarrays with properties
* palindrome behavior

If brute force is O(n²), two pointers often gives O(n).

---

# Interview Questions (With Answers)

### Q1. When do we prefer fast-slow over left-right?

* Fast–slow → linked list cycle / middle
* Left–right → arrays/strings

---

### Q2. Does two pointers always require sorted input?

No.
But sorted input **simplifies reasoning** in many problems.

---

### Q3. How is sliding window different from two pointers?

Sliding window:

* focuses on window property
* usually both pointers move forward

Two pointers (general):

* may move opposite directions

---

### Q4. Why does 3Sum use sorting first?

To leverage:

* left-right shrinking
* duplicate skipping
* O(n²) complexity instead of O(n³)

---

### Q5. What’s time complexity typical for two pointers?

Often **O(n)** or **O(n log n)** (if sorting is included).

---

# LeetCode Problems to Practice (Two Pointers)

* [https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
* [https://leetcode.com/problems/3sum/](https://leetcode.com/problems/3sum/)
* [https://leetcode.com/problems/container-with-most-water/](https://leetcode.com/problems/container-with-most-water/)
* [https://leetcode.com/problems/move-zeroes/](https://leetcode.com/problems/move-zeroes/)
* [https://leetcode.com/problems/remove-duplicates-from-sorted-array/](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
* [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)
* [https://leetcode.com/problems/valid-palindrome/](https://leetcode.com/problems/valid-palindrome/)
* [https://leetcode.com/problems/merge-sorted-array/](https://leetcode.com/problems/merge-sorted-array/)
* [https://leetcode.com/problems/sort-colors/](https://leetcode.com/problems/sort-colors/)
* [https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

---

# Summary

* Two pointers reduce nested loops to linear scans
* Several sub-patterns:

  * left–right
  * fast–slow
  * sliding window
  * bucket/segregation
  * string based
* Powerful for:

  * sorted arrays
  * palindrome checks
  * duplicates removal
  * subarray problems
  * linked lists
* Key skills: move correct pointer based on condition

---
