# Range Product Queries of Powers – LeetCode #2438

## Problem Statement

Given a positive integer `n`, construct a 0-indexed array `powers` consisting of the **minimum number of powers of 2** that sum to `n`. The array is sorted in non-decreasing order and is **unique**.

You are also given a 0-indexed 2D integer array `queries`, where `queries[i] = [leftᵢ, rightᵢ]`.

For each query, find the product of all elements `powers[j]` where `leftᵢ <= j <= rightᵢ`.

Since the product can be large, return each answer modulo **10⁹ + 7**.

---

## Quick Revision

- Convert `n` into a **list of powers of two** present in its binary form.
- For each query, multiply the relevant segment of the `powers` list, taking modulo `10⁹ + 7`.
- Time complexity:  
  - **O(31)** to extract powers of two (since `n <= 10⁹` fits in 31 bits).  
  - **O(Q * L)** for queries, where `L` is query range size.

---

## Java Code

```java
class Solution {
    public int[] productQueries(int n, int[][] queries) {
        int MOD = 1_000_000_007;
        int[] resultArray = new int[queries.length];
        List<Integer> list = new ArrayList<>();
        
        // Extract powers of two from n
        for (int i = 0; i < 31; i++) {
            if ((n & (1 << i)) != 0) {
                list.add(1 << i);
            }
        }

        // Answer each query
        for (int i = 0; i < queries.length; i++) {
            int start = queries[i][0];
            int end = queries[i][1];
            long result = 1;
            for (int j = start; j <= end; j++) {
                result = (result * list.get(j)) % MOD;
            }
            resultArray[i] = (int) result;
        }
        return resultArray;
    }
}
