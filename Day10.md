# Reordered Power of 2 – LeetCode #869

## Problem Statement

You are given an integer `n`. We can reorder the digits in any order (including the original order) as long as the **leading digit is not zero**.

Return `true` if and only if it is possible to reorder the digits so that the resulting number is a **power of two**.

---

## Quick Revision

- **Idea**: Two numbers are reorder-equivalent if their **sorted digit strings** are the same.
- Precompute all powers of two up to `2^30` (since `n ≤ 10^9`), sort their digits, and compare.
- Sorting ensures that reordering is reduced to simple string equality.

---

## Java Code

```java
class Solution {
    public boolean reorderedPowerOf2(int n) {
        String target = sortedString(n);
        for (int i = 0; i < 31; i++) {
            if (sortedString(1 << i).equals(target)) return true;
        }
        return false;
    }

    private String sortedString(int x) {
        char[] arr = String.valueOf(x).toCharArray();
        Arrays.sort(arr);
        return new String(arr);
    }
}
