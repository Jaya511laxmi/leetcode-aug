# Maximum Fruits Harvested After at Most K Steps – LeetCode #2106

## Problem Statement

Fruits are available at some positions on an infinite x-axis. You are given a 2D integer array `fruits` where `fruits[i] = [positioni, amounti]` depicts `amounti` fruits at the position `positioni`.  
`fruits` is sorted by `positioni` in ascending order, and each `positioni` is unique.

You are also given an integer `startPos` and an integer `k`.  
Initially, you are at the position `startPos`. From any position, you can either walk to the left or right. It takes one step to move one unit on the x-axis, and you can walk at most `k` steps in total.

For every position you reach, you harvest **all the fruits at that position**, and the fruits will disappear from that position.

**Return the maximum total number of fruits you can harvest.**

---

## Quick Revision

- Use a **sliding window** across the `fruits` array.
- Always check if window `[i, j]` is **reachable within `k` steps**.
- Steps needed to reach both ends of window:
  - totalSteps = (fruits[j][0] - fruits[i][0]) + min(abs(startPos - fruits[i][0]), abs(startPos - fruits[j][0]))
- Track running sum `s` of fruits in window and update max.

---

## Java Code

```java
class Solution {
  public int maxTotalFruits(int[][] fruits, int startPos, int k) {
      int ans = 0, s = 0;
      for (int i = 0, j = 0; j < fruits.length; ++j) {
          int pj = fruits[j][0], fj = fruits[j][1];
          s += fj;

          while (i <= j
              && pj - fruits[i][0]
                      + Math.min(Math.abs(startPos - fruits[i][0]), Math.abs(startPos - pj))
                  > k) {
              s -= fruits[i++][1];
          }

          ans = Math.max(ans, s);
      }
      return ans;
  }
}
 
