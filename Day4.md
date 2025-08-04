# Fruit Into Baskets – LeetCode #904

## Problem Statement

You're visiting a farm with a row of fruit trees represented by an array `fruits`, where `fruits[i]` is the type of fruit from the i-th tree.

You want to collect the **maximum number of fruits** under the following rules:
- You have **only two baskets**.
- Each basket can hold **only one type** of fruit.
- You must pick **one fruit per tree**, moving **strictly to the right**.
- Once you encounter a tree with a third type of fruit, you must stop.

**Return the maximum number of fruits you can collect.**

---

## Quick Revision

- You can only carry **2 types of fruits**.
- Use a **sliding window** to keep track of the current valid segment.
- Shrink window when the number of distinct fruits exceeds 2.

Logic:
1. Expand the right end of the window (`end`).
2. Add fruit to the map and count occurrences.
3. If more than 2 fruit types, shrink the window from the left (`start`).
4. Track max length of window at every step.

---

## Java Code

```java
class Solution {
    public int totalFruit(int[] fruits) {
        int start = 0, end = 0;
        int n = fruits.length, maxLen = 0;
        Map<Integer, Integer> map = new HashMap<>();

        while (end < n) {
            map.put(fruits[end], map.getOrDefault(fruits[end], 0) + 1);

            while (map.size() >= 3) {
                map.put(fruits[start], map.get(fruits[start]) - 1);
                if (map.get(fruits[start]) == 0) map.remove(fruits[start]);
                start++;
            }

            int currLen = end - start + 1;
            maxLen = Math.max(maxLen, currLen);
            end++;
        }

        return maxLen;
    }
}
