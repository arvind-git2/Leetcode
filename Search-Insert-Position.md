# Search Insert Position

**Platform:** LeetCode  
**Problem Number:** 35  
**Difficulty:** Easy

---

# 1. Intuition

We need to find the position of a given `target` in a **sorted array**.

If the target is already present, we return its index.

If the target is not present, we need to return the index where it should be inserted so that the array remains sorted.

A brute-force approach would traverse the array from left to right and compare every element with the target. This takes **O(n)** time.

Since the array is already sorted, we can use **Binary Search** to reduce the search time to **O(log n)**.

The important observation is that when the target is not found, the `low` pointer eventually reaches the exact position where the target should be inserted.

---

# 2. Approach

## Algorithm

1. Initialize two pointers:
   - `low = 0`
   - `high = nums.size() - 1`
2. Calculate the middle index:
   ```cpp
   mid = low + (high - low) / 2;
   ```
3. Compare `nums[mid]` with `target`.
   - If `nums[mid] == target`, return `mid`.
   - If `nums[mid] < target`, search in the right half:
     ```cpp
     low = mid + 1;
     ```
   - If `nums[mid] > target`, search in the left half:
     ```cpp
     high = mid - 1;
     ```
4. Continue until `low > high`.
5. If the target is not found, return `low` because it represents the correct insertion position.

---

## Example

### Input

```text
nums = [1,3,5,6]
target = 5
```

### Dry Run

Initially:

```text
low = 0
high = 3
mid = 1
```

Array:

```text
Index:  0  1  2  3
        1  3  5  6
           ↑
          mid
```

Since:

```text
nums[mid] = 3
target = 5
```

and:

```text
3 < 5
```

we search in the right half.

```cpp
low = mid + 1;
```

Now:

```text
low = 2
high = 3
```

Calculate `mid`:

```text
mid = 2
```

Now:

```text
nums[mid] = 5
target = 5
```

Since both are equal:

```cpp
return mid;
```

Therefore:

```text
Output = 2
```

---

### Input

```text
nums = [1,3,5,6]
target = 2
```

### Dry Run

Initially:

```text
low = 0
high = 3
mid = 1
```

Array:

```text
Index:  0  1  2  3
        1  3  5  6
           ↑
          mid
```

Since:

```text
3 > 2
```

we search in the left half.

```cpp
high = mid - 1;
```

Now:

```text
low = 0
high = 0
```

Calculate:

```text
mid = 0
```

Now:

```text
nums[mid] = 1
target = 2
```

Since:

```text
1 < 2
```

we move to the right:

```cpp
low = mid + 1;
```

Now:

```text
low = 1
high = 0
```

The condition:

```cpp
low <= high
```

is false, so the loop terminates.

Finally:

```cpp
return low;
```

Therefore:

```text
Output = 1
```

The target `2` should be inserted at index `1`.

---

### Input

```text
nums = [1,3,5,6]
target = 7
```

Since `7` is greater than all elements, the search eventually moves `low` beyond the last element.

```text
low = 4
high = 3
```

Since:

```text
low > high
```

the loop terminates.

We return:

```cpp
return low;
```

Therefore:

```text
Output = 4
```

The target `7` should be inserted at the end of the array.

---

## Time Complexity

- **Time:** `O(log n)`
- **Space:** `O(1)`

---

# 3. Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int searchInsert(vector<int>& nums, int target) {
    int low = 0;
    int high = nums.size() - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (nums[mid] == target) {
            return mid;
        }
        else if (nums[mid] < target) {
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }

    return low;
}

int main() {
    vector<int> nums = {1, 3, 5, 6};
    int target = 2;

    cout << searchInsert(nums, target);

    return 0;
}
```

---

## Complexity Analysis

| **Complexity** | **Value** |
|---|---|
| Time | **O(log n)** |
| Space | **O(1)** |

---

## Key Concepts

- Binary Search
- Sorted Array
- Two Pointer Technique
- Index Tracking
- Search Space Reduction
- `low` and `high` pointers
- `mid` calculation
- Insertion Position
- Time Optimization
- Constant Space
