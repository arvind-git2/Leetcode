# Contains Duplicate II

**Platform:** LeetCode  
**Problem Number:** 219  
**Difficulty:** Easy

---

# 1. Intuition

We need to determine whether the array contains two identical elements whose indices differ by at most `k`.

A brute-force approach would compare every pair of elements, but this takes **O(n²)** time, which is inefficient for large arrays.

Instead, we store the **most recent index** of every element in a hash map (`unordered_map`). Whenever we encounter an element again, we calculate the distance between its current index and its previous index. If this distance is less than or equal to `k`, we return `true`.

This allows us to solve the problem in **O(n)** time.

---

# 2. Approach

## Algorithm

1. Create an `unordered_map<int, int>` to store the last index of each element.
2. Traverse the array from left to right.
3. For each element:
   - Check if it already exists in the map.
   - If it exists, calculate:
     ```
     currentIndex - previousIndex
     ```
   - If the difference is less than or equal to `k`, return `true`.
4. Update the element's latest index in the map.
5. If no valid pair is found after the traversal, return `false`.

---

## Example

### Input

```text
nums = [1,2,3,1]
k = 3
```

| Index | Element | Previous Index | Distance | Result |
|------:|--------:|---------------:|---------:|--------|
|0|1|-|-|Store 0|
|1|2|-|-|Store 1|
|2|3|-|-|Store 2|
|3|1|0|3|3 ≤ 3 → Return true|

---

### Input

```text
nums = [1,2,3,1,2,3]
k = 2
```

| Duplicate | Distance |
|-----------|----------|
|1|3|
|2|3|
|3|3|

Since every distance is greater than `k`, the answer is **false**.

---

## Time Complexity

- **Time:** `O(n)`
- **Space:** `O(n)`

---

# 3. Code

```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> lastIndex;

        for (int i = 0; i < nums.size(); i++) {

            if (lastIndex.count(nums[i]) &&
                i - lastIndex[nums[i]] <= k) {
                return true;
            }

            lastIndex[nums[i]] = i;
        }

        return false;
    }
};
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(n)** |

---

## Key Concepts

- Hash Map (`unordered_map`)
- Index Tracking
- Single Pass Traversal
- Time Optimization
- Constant-Time Lookup
