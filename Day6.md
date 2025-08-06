# Fruits Into Baskets III – LeetCode #3479

## Problem Statement

You are given two arrays `fruits` and `baskets`, each of length `n`:

- `fruits[i]` represents the quantity of the i-th fruit type.
- `baskets[j]` represents the capacity of the j-th basket.

From **left to right**, place the fruits following these rules:

1. Each fruit must go into the **leftmost available basket** with **capacity ≥ quantity**.
2. Each basket can hold **only one type of fruit**.
3. If no basket can hold a fruit type, it remains **unplaced**.

**Return the number of fruit types that remain unplaced.**

---

## Quick Revision

- Each fruit must go into the **first suitable basket** (`capacity >= fruit[i]`).
- Once a basket is used, it **cannot be reused**.
- A **Segment Tree** is used to:
  - Find the leftmost basket with enough capacity.
  - Mark the basket as used (set capacity = 0).

---

## Java Code

```java
class Solution {
    int n;
    int[] seg;

    void Update(int p) {
        seg[p] = Math.max(seg[p << 1], seg[p << 1 | 1]);
    }

    void Build(int p, int l, int r, int[] baskets) {
        if (l == r) {
            seg[p] = baskets[l];
            return;
        }
        int mid = (l + r) >> 1;
        Build(p << 1, l, mid, baskets);
        Build(p << 1 | 1, mid + 1, r, baskets);
        Update(p);
    }

    void Assign(int x, int v, int p, int l, int r) {
        if (x < l || x > r) return;
        if (l == r) {
            seg[p] = v;
            return;
        }
        int mid = (l + r) >> 1;
        Assign(x, v, p << 1, l, mid);
        Assign(x, v, p << 1 | 1, mid + 1, r);
        Update(p);
    }

    int FirstLarger(int v, int p, int l, int r) {
        if (seg[p] < v) return r + 1;
        if (l == r) return r;
        int mid = (l + r) >> 1;
        int lf = FirstLarger(v, p << 1, l, mid);
        if (lf <= mid) return lf;
        return FirstLarger(v, p << 1 | 1, mid + 1, r);
    }

    public int numOfUnplacedFruits(int[] fruits, int[] baskets) {
        n = fruits.length;
        seg = new int[4 * n + 1];
        Build(1, 0, n - 1, baskets);
        int res = 0;
        for (int x : fruits) {
            int pos = FirstLarger(x, 1, 0, n - 1);
            if (pos == n) {
                res++;
            } else {
                Assign(pos, 0, 1, 0, n - 1);
            }
        }
        return res;
    }
}
