# Find the Maximum Number of Fruits Collected – LeetCode #3363

## Problem Statement

There is a game dungeon comprised of **n × n** rooms arranged in a grid.

You are given a 2D array `fruits` of size **n × n**, where `fruits[i][j]` represents the number of fruits in the room (i, j). Three children start at the corners of the grid:

- Child 1 at position **(0, 0)**
- Child 2 at position **(0, n - 1)**
- Child 3 at position **(n - 1, 0)**

All three children aim to reach the bottom-right corner cell **(n - 1, n - 1)** in exactly **n - 1 moves**. The moves are as follows:

- **Child 1** can move to (i+1, j), (i, j+1), or (i+1, j+1)
- **Child 2** can move to (i+1, j), (i+1, j-1), or (i+1, j+1)
- **Child 3** can move to (i, j+1), (i-1, j+1), or (i+1, j+1)

When a child enters a room, they collect all the fruits in it. If multiple children enter the same room, the fruits are collected **only once**.

**Return the maximum number of fruits** the children can collectively collect.

---

## Quick Revision

- **Three children**, each starting from a corner, must reach the bottom-right cell in **n-1 moves**.
- All children collect fruits along their path.
- If multiple children enter the same cell, fruits are counted **once only**.
- Use **Dynamic Programming** and **symmetry** (rotate matrix) to simulate optimal paths for each child independently.

---

## Java Code

```java
class Solution {

    public int maxCollectedFruits(int[][] fruits) {
        int n = fruits.length;
        int ans = 0;

        // First child's diagonal (0,0) → (1,1) → … → (n-1,n-1)
        for (int i = 0; i < n; ++i) {
            ans += fruits[i][i];
        }

        // Function to simulate one of the child's path using DP
        java.util.function.Supplier<Integer> dp = () -> {
            int[] prev = new int[n];
            int[] curr = new int[n];
            java.util.Arrays.fill(prev, Integer.MIN_VALUE);
            java.util.Arrays.fill(curr, Integer.MIN_VALUE);

            // Start from top-right corner
            prev[n - 1] = fruits[0][n - 1];

            for (int i = 1; i < n - 1; ++i) {
                for (int j = Math.max(n - 1 - i, i + 1); j < n; ++j) {
                    int best = prev[j];
                    if (j - 1 >= 0) best = Math.max(best, prev[j - 1]);
                    if (j + 1 < n) best = Math.max(best, prev[j + 1]);
                    curr[j] = best + fruits[i][j];
                }
                int[] temp = prev;
                prev = curr;
                curr = temp;
            }

            return prev[n - 1];
        };

        // Simulate for child 2 (top-right to bottom-right)
        ans += dp.get();

        // Transpose the matrix to simulate child 3 (bottom-left to bottom-right)
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int temp = fruits[j][i];
                fruits[j][i] = fruits[i][j];
                fruits[i][j] = temp;
            }
        }

        // Simulate for child 3 on transposed grid
        ans += dp.get();

        return ans;
    }
}
