# Dynamic Programming in 2D Grids

Dynamic Programming on grids deals with problems where you move through a **2D matrix** and must compute something optimally such as:

* number of paths
* minimum / maximum cost
* longest area / region
* constrained movement

The cell `(i, j)` usually depends on **some neighboring cells**, and thus we define:

```
dp[i][j] = best answer up to cell (i, j)
```

---

## Core Idea

Most 2D grid DP problems follow this intuition:

1. You start from a cell (typically `(0,0)` or top-left)
2. You can move in limited directions
3. You accumulate sum / cost / ways
4. You use DP to avoid recomputation

---

## Common movement directions in grid DP

* right `(i, j+1)`
* down `(i+1, j)`
* diagonal `(i+1, j+1)` — sometimes

We build recurrence based on allowed movements.

---

## Template for Grid DP Problems

1. define DP state
   `dp[i][j] = best answer up to cell (i, j)`

2. determine transitions
   depends on allowed moves

3. handle boundaries carefully

4. set base case at starting cell

5. compute answer at bottom-right (usually)

---

## 1. Counting Paths in Grid (Unique Paths)

Problem idea:
From `(0,0)` to `(n-1,m-1)` using only right and down moves.
Count number of ways.

---

### Recurrence

From `(i,j)` you can come from:

* top → `(i−1, j)`
* left → `(i, j−1)`

So:

```
dp[i][j] = dp[i-1][j] + dp[i][j-1]
```

Base:

```
dp[0][0] = 1
```

---

### Tabulation Code (C++)

```cpp
int uniquePaths(int m, int n) {
    vector<vector<int>> dp(m, vector<int>(n, 0));

    dp[0][0] = 1;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {

            if (i == 0 && j == 0) continue;

            int up = 0, left = 0;

            if (i > 0) up = dp[i-1][j];
            if (j > 0) left = dp[i][j-1];

            dp[i][j] = up + left;
        }
    }
    return dp[m-1][n-1];
}
```

---

## 2. Unique Paths with Obstacles

Same as above, but some cells are blocked.

```
1 → free cell
0 → obstacle
```

---

### Transition Adjustment

If obstacle exists → `dp[i][j] = 0`

---

### Code

```cpp
int uniquePathsWithObstacles(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<long long>> dp(m, vector<long long>(n, 0));

    if (grid[0][0] == 1) return 0;

    dp[0][0] = 1;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {

            if (grid[i][j] == 1) {
                dp[i][j] = 0;
                continue;
            }

            if (i == 0 && j == 0) continue;

            long long up = 0, left = 0;
            if (i > 0) up = dp[i-1][j];
            if (j > 0) left = dp[i][j-1];

            dp[i][j] = up + left;
        }
    }
    return dp[m-1][n-1];
}
```

---

## 3. Minimum Path Sum

Each cell has a cost.
Find minimum cost path from `(0,0)` to `(n-1,m-1)` moving only right/down.

---

### Recurrence

```
dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
```

Boundary handling is important.

---

### Code

```cpp
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> dp(m, vector<int>(n));

    dp[0][0] = grid[0][0];

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {

            if (i == 0 && j == 0) continue;

            int up = INT_MAX, left = INT_MAX;

            if (i > 0) up = dp[i-1][j];
            if (j > 0) left = dp[i][j-1];

            dp[i][j] = grid[i][j] + min(up, left);
        }
    }
    return dp[m-1][n-1];
}
```

---

## 4. Maximum Square of 1s (Largest Square Submatrix)

Find largest square consisting only of 1s.

---

### Recurrence

If cell is 1:

```
dp[i][j] = 1 + min(
    dp[i-1][j],     // up
    dp[i][j-1],     // left
    dp[i-1][j-1]    // diagonal
)
```

If cell is 0:

```
dp[i][j] = 0
```

---

### Code

```cpp
int maximalSquare(vector<vector<char>>& matrix) {
    int m = matrix.size(), n = matrix[0].size();
    vector<vector<int>> dp(m, vector<int>(n, 0));
    int ans = 0;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {

            if (matrix[i][j] == '1') {
                if (i == 0 || j == 0)
                    dp[i][j] = 1;
                else
                    dp[i][j] = 1 + min({ dp[i-1][j], dp[i][j-1], dp[i-1][j-1] });
            }

            ans = max(ans, dp[i][j]);
        }
    }
    return ans;
}
```

---

## Space Optimization in Grid DP

Observation:

Most formulas depend on:

* current row
* previous row

So instead of `dp[m][n]`, we can use:

```
vector<int> prev(n), cur(n);
```

---

Example for minimum path sum:

```cpp
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();

    vector<int> prev(n);

    for (int i = 0; i < m; i++) {
        vector<int> cur(n);

        for (int j = 0; j < n; j++) {
            if (i == 0 && j == 0) cur[j] = grid[0][0];
            else {
                int up = (i > 0) ? prev[j] : INT_MAX;
                int left = (j > 0) ? cur[j-1] : INT_MAX;
                cur[j] = grid[i][j] + min(up, left);
            }
        }
        prev = cur;
    }
    return prev[n-1];
}
```

Space reduced to **O(n)** instead of `O(n*m)`.

---

## Typical Grid DP Variations Asked in Interviews

* count paths
* obstacles in grids
* maximum/minimum path sum
* paths with constraints
* collect maximum coins
* DP with diagonals allowed
* DP with modulo operations
* DP with negative weights

---

## Common Mistakes in 2D Grid DP

* not handling boundary cells correctly
* accessing outside bounds
* starting base case incorrectly
* mixing recursion with tabulation unintentionally
* forgetting modulo when numbers are large
* assuming only right/down moves when not specified

---

## Interview Questions (With Answers)

### Q1. Why does DP work so well on grids?

Because each cell solution depends on smaller subgrid solutions.

### Q2. Why do we avoid recursion on grids sometimes?

Deep recursion may cause stack overflow and repeated recomputation.

### Q3. When is space optimization possible?

When each cell depends only on:

* current row
* previous row

### Q4. Is greedy applicable in grid path sums?

Rarely. You must consider future cells; DP ensures global optimality.

---

## LeetCode Problems on Grid DP

* [https://leetcode.com/problems/unique-paths/](https://leetcode.com/problems/unique-paths/)
* [https://leetcode.com/problems/unique-paths-ii/](https://leetcode.com/problems/unique-paths-ii/)
* [https://leetcode.com/problems/minimum-path-sum/](https://leetcode.com/problems/minimum-path-sum/)
* [https://leetcode.com/problems/coin-path/](https://leetcode.com/problems/coin-path/)
* [https://leetcode.com/problems/dungeon-game/](https://leetcode.com/problems/dungeon-game/)
* [https://leetcode.com/problems/cherry-pickup/](https://leetcode.com/problems/cherry-pickup/)
* [https://leetcode.com/problems/cherry-pickup-ii/](https://leetcode.com/problems/cherry-pickup-ii/)
* [https://leetcode.com/problems/maximal-square/](https://leetcode.com/problems/maximal-square/)
* [https://leetcode.com/problems/path-with-maximum-gold/](https://leetcode.com/problems/path-with-maximum-gold/)
* [https://leetcode.com/problems/minimum-falling-path-sum/](https://leetcode.com/problems/minimum-falling-path-sum/)

---

## Summary

* 2D grid DP is one of the most important DP categories

* Define `dp[i][j]` meaning clearly first

* Choose movement directions and base cases

* Handle boundaries carefully

* Optimize space when feasible

* Practice classic problems to build intuition

---

If you want next, I can write repo-ready guides for:

* Minimum Falling Path Sum (in depth)
* Cherry Pickup (hard grid DP explained)
* DP on triangles
* DP on grids with diagonals
* DP with backtracking vs DP distinction
