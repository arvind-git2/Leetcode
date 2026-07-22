# Search in Rotated Sorted Array

**Platform:** LeetCode  
**Problem Number:** 33  
**Difficulty:** Medium

---

# 1. Intuition

Since the array was originally sorted but may have been rotated, a normal binary search cannot be applied directly.

The key observation is that **at least one half of the array is always sorted**. By identifying the sorted half at every step, we can determine whether the target lies within that half or the other half, allowing us to eliminate half of the search space each iteration.

This preserves the efficiency of binary search and achieves the required **O(log n)** time complexity.

---

# 2. Approach

## Algorithm

1. Initialize two pointers:
   - `low = 0`
   - `high = n - 1`

2. While `low <= high`:
   - Compute the middle index.
   - If `nums[mid]` equals the target, return `mid`.

3. Determine which half is sorted:
   - **Left Half Sorted**
     - If `nums[low] <= nums[mid]`, the left half is sorted.
     - If the target lies between `nums[low]` and `nums[mid]`, search the left half.
     - Otherwise, search the right half.

   - **Right Half Sorted**
     - Otherwise, the right half is sorted.
     - If the target lies between `nums[mid]` and `nums[high]`, search the right half.
     - Otherwise, search the left half.

4. If the target is not found after the loop, return `-1`.

---

## Time Complexity

- **Time:** `O(log n)`
- **Space:** `O(1)`

---

# 3. Code

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (nums[mid] == target)
                return mid;

            // Left half is sorted
            if (nums[low] <= nums[mid]) {
                if (target >= nums[low] && target < nums[mid])
                    high = mid - 1;
                else
                    low = mid + 1;
            }
            // Right half is sorted
            else {
                if (target > nums[mid] && target <= nums[high])
                    low = mid + 1;
                else
                    high = mid - 1;
            }
        }

        return -1;
    }
};
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(log n)** |
| Space | **O(1)** |

---

## Key Takeaways

- Modified Binary Search
- Identifying the sorted half of a rotated array
- Efficient searching in rotated sorted arrays
- Constant extra space
