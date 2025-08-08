# Soup Servings – LeetCode #808

## Problem Statement

You have two soups **A** and **B**, each starting with `n` mL. On every turn, one of the following four serving operations is chosen **at random** (each with probability `0.25`):

1. Pour **100 mL from A** and **0 mL from B**
2. Pour **75 mL from A** and **25 mL from B**
3. Pour **50 mL from A** and **50 mL from B**
4. Pour **25 mL from A** and **75 mL from B**

**Rules:**
- If the requested amount exceeds what remains, pour **all that remains**.
- The process stops immediately when **either** soup is empty.
- **Goal:** Return the probability that soup A becomes empty **before** soup B, **plus half** the probability that both become empty at the same time.

**Accuracy requirement:** Your answer must be within `1e-5` of the actual value.

---

## Quick Revision

- **Observation:** Amounts are multiples of 25 mL → scale down `n` to units of 25 mL for faster computation.
- **Base cases:**
  - If **both** soups empty → return `0.5`
  - If **A** empty first → return `1.0`
  - If **B** empty first → return `0.0`
- **Optimization:** For large `n` (over ~5000 mL), the probability approaches **1** due to high chances of A running out first.
- **Approach:** **Memoized DFS** (Top-down DP) to explore all possible serving paths.

---

## Java Code

```java
class Solution {
    private Double[][] cache; // Memoization storage

    public double soupServings(int n) {
        // Large n behaves like probability 1
        if (n > 5000) return 1.0;

        // Convert mL → 25 mL units
        int units = (int) Math.ceil(n / 25.0);
        cache = new Double[units + 1][units + 1];

        return calcProb(units, units);
    }

    private double calcProb(int soupA, int soupB) {
        // Both soups empty → half probability
        if (soupA <= 0 && soupB <= 0) return 0.5;
        // A empty first
        if (soupA <= 0) return 1.0;
        // B empty first
        if (soupB <= 0) return 0.0;

        // Check memoized value
        if (cache[soupA][soupB] != null) return cache[soupA][soupB];

        // Recursively compute probability
        double prob = 0.25 * (
            calcProb(soupA - 4, soupB) +      // 100A, 0B
            calcProb(soupA - 3, soupB - 1) +  // 75A, 25B
            calcProb(soupA - 2, soupB - 2) +  // 50A, 50B
            calcProb(soupA - 1, soupB - 3)    // 25A, 75B
        );

        cache[soupA][soupB] = prob;
        return prob;
    }
}
