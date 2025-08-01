# Pascal's Triangle – LeetCode #118

## Problem Statement

Given an integer `numRows`, generate the first `numRows` of Pascal's Triangle.

In Pascal's Triangle:
- Each number is the **sum of the two numbers directly above it**.
- The triangle starts with `1` at the top.
---

## Quick Revision

- First row is always `[1]`.
- Every new row:
  - Starts and ends with `1`.
  - Middle values: **Sum of two numbers directly above**.

Formula:  
`currentRow[j] = prevRow[j-1] + prevRow[j]` (for `1 <= j < i`)

---

## Java Code 

```java
class Solution {
    public List<List<Integer>> generate(int n) {
        List<List<Integer>> ans = new ArrayList<>();
        ans.add(Collections.singletonList(1)); // First row

        for (int i = 1; i < n; i++) {
            List<Integer> currRow = new ArrayList<>();
            currRow.add(1); // Start with 1

            List<Integer> prevRow = ans.get(i - 1);
            for (int j = 0; j < i - 1; j++) {
                int num = prevRow.get(j) + prevRow.get(j + 1); // Sum above two
                currRow.add(num);
            }

            currRow.add(1); // End with 1
            ans.add(currRow); // Add to result
        }

        return ans;
    }
}
