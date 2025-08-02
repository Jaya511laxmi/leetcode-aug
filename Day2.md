# Rearranging Fruits (Hard) – LeetCode 2561 

## Problem Statement

You are given two baskets `basket1` and `basket2`, each with `n` fruits. Your goal is to make both baskets equal by repeatedly swapping any fruit from `basket1[i]` with `basket2[j]`.

- **Swap Cost**: `min(basket1[i], basket2[j])`
- **Equal Baskets**: After sorting, both arrays must be identical.
- **Return**: Minimum total cost to make baskets equal, or `-1` if impossible.


## Approach

1. **Count Frequencies** of all fruits in both baskets.
2. **Check Feasibility**: For each fruit, `(count in basket1 + count in basket2) % 2` must be `0`.
3. **Prepare Swap Lists**:
   - Add extra items (half the imbalance) from one basket to a list.
4. **Sort**:
   - One list ascending, one descending for minimal cost pairing.
5. **Calculate Cost**:
   - For each swap, cost = `min(direct swap, 2 * globally smallest fruit)`

---

## Java Code

```java
class Solution {
    public long minCost(int[] basket1, int[] basket2) {
        int n = basket1.length;
        Map<Integer, Integer> map1 = new HashMap<>();
        Map<Integer, Integer> map2 = new HashMap<>();
        int minVal = Integer.MAX_VALUE;

        for(int i = 0; i < n; i++){
            map1.put(basket1[i], map1.getOrDefault(basket1[i], 0) + 1);
            map2.put(basket2[i], map2.getOrDefault(basket2[i], 0) + 1);
            minVal = Math.min(minVal, Math.min(basket1[i], basket2[i]));
        }

        List<Integer> swapList1 = new ArrayList<>();
        for(int key: map1.keySet()){
            int c1 = map1.get(key), c2 = map2.getOrDefault(key, 0);
            if((c1 + c2) % 2 == 1) return -1;
            for(int i = 0; i < (c1 - c2) / 2; i++) swapList1.add(key);
        }

        List<Integer> swapList2 = new ArrayList<>();
        for(int key: map2.keySet()){
            int c1 = map1.getOrDefault(key, 0), c2 = map2.get(key);
            if((c1 + c2) % 2 == 1) return -1;
            for(int i = 0; i < (c2 - c1) / 2; i++) swapList2.add(key);
        }

        Collections.sort(swapList1);
        Collections.sort(swapList2, (a, b) -> b - a);

        long res = 0;
        for(int i = 0; i < swapList1.size(); i++) {
            res += Math.min(2 * minVal, Math.min(swapList1.get(i), swapList2.get(i)));
        }

        return res;
    }
}
