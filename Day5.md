# Fruits Into Baskets II – LeetCode #3477

## Problem Statement

You are given two arrays of integers `fruits` and `baskets`, both of length `n`:

- `fruits[i]` is the quantity of the i-th type of fruit.
- `baskets[j]` is the capacity of the j-th basket.

Place the fruits **from left to right** following these rules:

1. Each fruit type must be placed in the **leftmost available basket** with **capacity ≥ fruit quantity**.
2. Each basket can hold **only one type** of fruit.
3. If a fruit type **cannot be placed** in any basket, it remains **unplaced**.

**Return the number of fruit types that remain unplaced.**

---

## Quick Revision

- **Greedy approach** — For each fruit, scan from the **leftmost basket** for a suitable one.
- Once a basket is used, **set its capacity to 0** to mark it unavailable.
- After placing all fruits, **count how many remained unplaced**.

---

## Java Code

```java
class Solution {
    public int numOfUnplacedFruits(int[] fruits, int[] baskets) {
        for (int i = 0; i < fruits.length; i++) {
            for (int j = 0; j < baskets.length; j++) {
                if (fruits[i] <= baskets[j]) {
                    baskets[j] = 0; // mark basket as used
                    break;
                }
            }
        }

        int ans = 0;
        for (int i = 0; i < fruits.length; i++) {
            if (fruits[i] != 0) {
                boolean placed = false;
                for (int j = 0; j < baskets.length; j++) {
                    if (fruits[i] <= baskets[j]) {
                        placed = true;
                        break;
                    }
                }
                if (!placed) ans++;
            }
        }

        return ans;
    }
}
