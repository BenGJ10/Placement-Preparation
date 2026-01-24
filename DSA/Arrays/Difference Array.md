# Difference Array

A **Difference Array** is an optimization technique used to handle **multiple range update operations efficiently** on an array.

Instead of updating every element in a range `[l, r]` (which is O(n) per update), the difference array allows each update in **O(1)** time.

The final array is obtained by taking the **prefix sum** of the difference array.

---

## Why Difference Array Exists

Consider this problem:

> Given an array of size `n`, perform `q` range updates:
> add `x` to all elements in range `[l, r]`.

### Naive Approach

For each update:

```cpp
for (int i = l; i <= r; i++)
    a[i] += x;
```

Time complexity:

```
O(n × q)   // too slow
```

Difference array reduces this to:

```
O(q + n)
```

---

## Core Idea

Instead of updating the whole range:

* mark where the update **starts**
* mark where the update **ends**

---

## Definition

Given original array `a[]`, the **difference array** `diff[]` is defined as:

```
diff[0] = a[0]
diff[i] = a[i] − a[i−1]   for i ≥ 1
```

Conversely:

```
a[i] = diff[0] + diff[1] + ... + diff[i]
```

That is, **original array = prefix sum of diff array**.

---

## Range Update Using Difference Array

To add value `x` to range `[l, r]`:

```
diff[l]     += x
diff[r + 1] -= x
```

That’s it.

After processing all updates, take prefix sum of `diff` to rebuild the final array.

---

## Why This Works (Intuition)

When prefix sum is applied:

* from index `l` onward → value `x` starts getting added
* from index `r + 1` onward → value `x` is cancelled

So indices `[l, r]` get incremented by `x`.

---

## Step-by-Step Example

Initial array (n = 6):

```
a = [0, 0, 0, 0, 0, 0]
```

Operations:

```
add 5 to [1, 3]
add 2 to [2, 5]
```

### Build diff array

Initialize:

```
diff = [0, 0, 0, 0, 0, 0, 0]   // size n+1
```

Operation 1:

```
diff[1] += 5
diff[4] -= 5
```

Operation 2:

```
diff[2] += 2
diff[6] -= 2
```

Final diff:

```
[0, 5, 2, 0, -5, 0, -2]
```

### Prefix sum to recover array

```
a[0] = 0
a[1] = 5
a[2] = 7
a[3] = 7
a[4] = 2
a[5] = 2
```

---

## Implementation in C++

### Apply Multiple Range Updates

```cpp
vector<int> rangeUpdate(int n, vector<vector<int>>& updates) {
    vector<int> diff(n + 1, 0);

    for (auto& u : updates) {
        int l = u[0];
        int r = u[1];
        int val = u[2];

        diff[l] += val;
        if (r + 1 < n)
            diff[r + 1] -= val;
    }

    vector<int> a(n);
    a[0] = diff[0];

    for (int i = 1; i < n; i++)
        a[i] = a[i - 1] + diff[i];

    return a;
}
```

---

### Convert Existing Array to Difference Array

```cpp
vector<int> buildDiffArray(vector<int>& a) {
    int n = a.size();
    vector<int> diff(n);

    diff[0] = a[0];
    for (int i = 1; i < n; i++)
        diff[i] = a[i] - a[i - 1];

    return diff;
}
```

---

## Difference Array + Prefix Sum Relationship

This is important:

| Technique        | Purpose                             |
| ---------------- | ----------------------------------- |
| Difference Array | Fast range updates                  |
| Prefix Sum       | Fast range queries / reconstruction |

Difference array is essentially **inverse of prefix sum**.

---

## Difference Array in 2D (Advanced)

Used for **submatrix updates**.


### 2D Range Update Trick

To add `val` to submatrix `(r1,c1)` → `(r2,c2)`:

```
diff[r1][c1]     += val
diff[r1][c2+1]   -= val
diff[r2+1][c1]   -= val
diff[r2+1][c2+1] += val
```

Then:

* prefix sum row-wise
* prefix sum column-wise

---

### 2D Difference Array Skeleton

```cpp
vector<vector<int>> diff(m+1, vector<int>(n+1, 0));

diff[r1][c1] += val;
diff[r1][c2+1] -= val;
diff[r2+1][c1] -= val;
diff[r2+1][c2+1] += val;
```

---

## When to Use Difference Array

Use difference array when:

* many **range updates**
* final values needed after all updates
* no intermediate queries required
* constraints are large

Avoid when:

* frequent point queries are required mid-way
* both update and query operations are mixed (use Fenwick / Segment Tree)

---

## Common Mistakes

* forgetting `r + 1` boundary check
* not using array size `n + 1`
* applying prefix sum incorrectly
* mixing prefix sum logic with diff updates
* integer overflow (use `long long`)

---

## Difference Array vs Segment Tree

| Feature      | Difference Array   | Segment Tree |
| ------------ | ------------------ | ------------ |
| Range update | O(1)               | O(log n)     |
| Point query  | O(n) (after build) | O(log n)     |
| Mixed ops    | No                 | Yes          |
| Simplicity   | Very simple        | Complex      |
| Memory       | Low                | Higher       |

---

## Interview Questions

### Q1. Why difference array updates are O(1)?

Because only two indices (`l` and `r+1`) are modified per update.

---

### Q2. Why prefix sum is required after updates?

Because diff array stores **changes**, not final values.

---

### Q3. Can difference array handle negative updates?

Yes. Add negative values exactly the same way.

---

### Q4. What if updates are dynamic with queries?

Difference array alone is insufficient.
Use Fenwick Tree or Segment Tree.

---

### Q5. Why size is n+1?

To safely apply `diff[r+1] -= val` without overflow.

---

## LeetCode Problems Using Difference Array

* [https://leetcode.com/problems/corporate-flight-bookings/](https://leetcode.com/problems/corporate-flight-bookings/)
* [https://leetcode.com/problems/range-addition/](https://leetcode.com/problems/range-addition/)
* [https://leetcode.com/problems/car-pooling/](https://leetcode.com/problems/car-pooling/)
* [https://leetcode.com/problems/shifting-letters/](https://leetcode.com/problems/shifting-letters/)
* [https://leetcode.com/problems/matrix-block-sum/](https://leetcode.com/problems/matrix-block-sum/)
* [https://leetcode.com/problems/maximum-sum-obtained-of-any-permutation/](https://leetcode.com/problems/maximum-sum-obtained-of-any-permutation/)

---

## Summary

* Difference array optimizes **range updates**

* Each update costs O(1)

* Final array obtained via prefix sum

* Very useful for batch updates

* Closely related to prefix sum technique

* Essential for large constraints problems

---
