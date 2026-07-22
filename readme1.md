# Product of Array Except Self

**Platform:** LeetCode  
**Problem Number:** 238  
**Difficulty:** Medium

---

# 1. Intuition

For every index `i`, we need the product of all elements except `nums[i]`.

A brute-force solution would multiply every other element for each index, resulting in **O(n²)** time complexity, which is inefficient.

Using division is not allowed, so instead we compute:

- The product of all elements to the **left** of each index (Prefix Product).
- The product of all elements to the **right** of each index (Suffix Product).

Multiplying these two values gives the required product for each index.

This approach satisfies the required **O(n)** time complexity and **O(1)** extra space (excluding the output array).

---

# 2. Approach

## Algorithm

1. Create an output array `answer` initialized with `1`.
2. Traverse the array from left to right:
   - Maintain a running prefix product.
   - Store the prefix product in `answer[i]`.
3. Traverse the array from right to left:
   - Maintain a running suffix product.
   - Multiply `answer[i]` by the suffix product.
4. Return the output array.

---

## Example

**Input**

```text
nums = [1,2,3,4]
```

### Prefix Pass

| Index | Prefix | Answer |
|------:|--------:|--------:|
|0|1|1|
|1|1|1|
|2|2|2|
|3|6|6|

Answer after prefix pass:

```text
[1,1,2,6]
```

### Suffix Pass

| Index | Suffix | Updated Answer |
|------:|--------:|---------------:|
|3|1|6|
|2|4|8|
|1|12|12|
|0|24|24|

Final Output

```text
[24,12,8,6]
```

---

## Time Complexity

- **Time:** `O(n)`
- **Space:** `O(1)` *(excluding the output array)*

---

## 3. Code

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> answer(n, 1);

    int prefix = 1;
    for (int i = 0; i < n; i++) {
        answer[i] = prefix;
        prefix *= nums[i];
    }

    int suffix = 1;
    for (int i = n - 1; i >= 0; i--) {
        answer[i] *= suffix;
        suffix *= nums[i];
    }

    return answer;
}

int main() {
    vector<int> nums = {1, 2, 3, 4};

    vector<int> result = productExceptSelf(nums);

    for (int value : result)
        cout << value << " ";

    return 0;
}
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(1)** *(excluding output array)* |

---

## Key Concepts

- Prefix Product
- Suffix Product
- Array Traversal
- Space Optimization
- No Division
- O(n) Time Complexity
