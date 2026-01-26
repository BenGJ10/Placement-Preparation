# Binary Search on Answers

**Binary Search on Answers** is a technique where we **binary search over the range of possible answers**, not directly over array indices.

It is used when:

* the answer is numeric (minimum / maximum / threshold)
* checking a candidate answer is **possible or not**
* the feasibility function is **monotonic**

This is one of the **most important interview patterns**.

---

## Core Idea

Instead of searching for an element, we search for the **best possible answer**.

We define a function:

```
isPossible(mid) → true / false
```

Such that:

* for some answers → false
* after a point → always true
  OR
* for some answers → true
* after a point → always false

This **monotonic behavior** allows binary search.

---

## The Key Requirement: Monotonicity

Binary search on answers works only if:

```
false false false true true true
```

OR

```
true true true false false false
```

If feasibility flips randomly, binary search **cannot be applied**.

---

## General Template

```cpp
int low = minimum_possible_answer;
int high = maximum_possible_answer;
int ans = -1;

while (low <= high) {
    int mid = low + (high - low) / 2;

    if (isPossible(mid)) {
        ans = mid;          // mid is a valid answer
        high = mid - 1;     // try to find better (smaller) answer
    } else {
        low = mid + 1;      // mid not valid, increase
    }
}

return ans;
```

For **maximum answer problems**, directions reverse.

---

## How to Identify Binary Search on Answers

Look for phrases like:

* minimum / maximum possible value
* smallest / largest such that
* minimize the maximum
* maximize the minimum
* capacity / speed / time / distance
* can we do it in X?
* allocation / partitioning problems

If brute force is O(n × answer), this technique usually applies.

---

## Step-by-Step Thinking Process (Interview Gold)

1. Identify **search space of answers**
2. Define **isPossible(x)**
3. Check monotonicity
4. Apply binary search
5. Adjust bounds correctly

Most candidates fail at **step 2 and 4**.

---

## Classic Problem Patterns


## 1. Minimum Value That Satisfies a Condition

### Example: Square Root of a Number (Integer)

Find floor(sqrt(n)).

---

### Search Space

```
low = 0
high = n
```

---

### Feasibility Function

```
mid * mid <= n
```

---

### Code

```cpp
int mySqrt(int x) {
    long long low = 0, high = x;
    int ans = 0;

    while (low <= high) {
        long long mid = low + (high - low) / 2;

        if (mid * mid <= x) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```

---

## 2. Minimize the Maximum (Partition Problems)

### Example: Split Array Largest Sum

Split array into `k` subarrays such that **maximum subarray sum is minimized**.

---

### Search Space

```
low = max element
high = sum of all elements
```

---

### isPossible(mid)

Check if we can split array into ≤ k parts with max sum ≤ mid.

---

### Code

```cpp
bool canSplit(vector<int>& a, int k, int maxSum) {
    int parts = 1;
    long long curr = 0;

    for (int x : a) {
        if (curr + x > maxSum) {
            parts++;
            curr = x;
        } else {
            curr += x;
        }
    }
    return parts <= k;
}

int splitArray(vector<int>& a, int k) {
    int low = *max_element(a.begin(), a.end());
    int high = accumulate(a.begin(), a.end(), 0);
    int ans = high;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (canSplit(a, k, mid)) {
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```

---

## 3. Maximum Value That Satisfies a Condition

### Example: Aggressive Cows (Maximize Minimum Distance)

---

### Search Space

```
low = 1
high = max_position - min_position
```

---

### isPossible(mid)

Can we place cows such that minimum distance ≥ mid?

---

### Code

```cpp
bool canPlace(vector<int>& stalls, int cows, int dist) {
    int placed = 1;
    int last = stalls[0];

    for (int i = 1; i < stalls.size(); i++) {
        if (stalls[i] - last >= dist) {
            placed++;
            last = stalls[i];
        }
    }
    return placed >= cows;
}

int aggressiveCows(vector<int>& stalls, int cows) {
    sort(stalls.begin(), stalls.end());

    int low = 1;
    int high = stalls.back() - stalls.front();
    int ans = 0;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (canPlace(stalls, cows, mid)) {
            ans = mid;
            low = mid + 1;   // maximize
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```

---

## 4. Minimum Speed / Capacity Problems

### Example: Koko Eating Bananas

Find minimum eating speed.

---

### Search Space

```
low = 1
high = max pile size
```

---

### isPossible(mid)

Compute total hours needed at speed `mid`.

---

### Code

```cpp
bool canEat(vector<int>& piles, int h, int speed) {
    long long hours = 0;
    for (int x : piles)
        hours += (x + speed - 1) / speed;

    return hours <= h;
}

int minEatingSpeed(vector<int>& piles, int h) {
    int low = 1;
    int high = *max_element(piles.begin(), piles.end());
    int ans = high;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (canEat(piles, h, mid)) {
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```

---

## Binary Search Direction Rules

| Problem Type | If isPossible(mid) is true |
| ------------ | -------------------------- |
| Find minimum | move `high = mid - 1`      |
| Find maximum | move `low = mid + 1`       |

Wrong direction = wrong answer.

---

## Common Search Space Mistakes

* choosing incorrect `low`
* choosing incorrect `high`
* not including all valid answers
* integer overflow
* wrong monotonic assumption

---

## Time Complexity Pattern

```
Binary Search on Answer:
O(log(answer_range) × cost_of_isPossible)
```

Often:

* `isPossible` → O(n)
* overall → O(n log range)

This is why brute force TLEs but this passes.

---

## Common Mistakes (Very Important)

* writing `isPossible` incorrectly
* breaking monotonicity unintentionally
* forgetting to update answer
* infinite loop due to bad bounds
* using `int` instead of `long long`
* wrong mid update direction

---

## Interview Questions

### Q1. Why binary search on answers works?

Because feasibility function is **monotonic**.

---

### Q2. How is it different from normal binary search?

Normal binary search searches indices.
This searches the **solution space**.

---

### Q3. When should this be preferred?

When brute force checks answers linearly and feasibility is monotonic.

---

### Q4. What if feasibility is not monotonic?

Binary search cannot be applied.

---

### Q5. How to debug wrong answers?

Print:

* low, mid, high
* feasibility result
* check boundary values manually

---

## LeetCode Problems for Binary Search on Answers

* [https://leetcode.com/problems/koko-eating-bananas/](https://leetcode.com/problems/koko-eating-bananas/)
* [https://leetcode.com/problems/split-array-largest-sum/](https://leetcode.com/problems/split-array-largest-sum/)
* [https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)
* [https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)
* [https://leetcode.com/problems/aggressive-cows/](https://leetcode.com/problems/aggressive-cows/) (GFG / classic)
* [https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/)
* [https://leetcode.com/problems/allocate-books/](https://leetcode.com/problems/allocate-books/)
* [https://leetcode.com/problems/painters-partition-problem/](https://leetcode.com/problems/painters-partition-problem/)
* [https://leetcode.com/problems/minimize-max-distance-to-gas-station/](https://leetcode.com/problems/minimize-max-distance-to-gas-station/)

---

## Summary

* Binary Search on Answers searches **solution space**

* Requires **monotonic feasibility**

* Extremely common in hard interview problems

* Pattern:

  * define search space
  * define isPossible
  * binary search

* Precision in boundaries and direction is crucial

---

