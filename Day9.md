# Power of Two – LeetCode #231

## Problem Statement

Given an integer `n`, return `true` if it is a **power of two**, otherwise return `false`.

An integer `n` is a power of two if there exists an integer `x` such that:
`n == 2^x`

---

## Quick Revision

- A positive power of two in binary has **exactly one bit set** (e.g., 1 → `0001`, 2 → `0010`, 4 → `0100`).
- Check this property using the bitwise trick:
  `n > 0 && (n & (n - 1)) == 0 `
- This works because subtracting `1` from a power of two flips all bits after the set bit, and `&` will clear it.

---

## Java Code

```java
class Solution {
  public boolean isPowerOfTwo(int n) {
      if (n > 0 && (n & (n - 1)) == 0) 
          return true;
      else 
          return false;
  }
}
