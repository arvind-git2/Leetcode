# Largest Rectangle in Histogram

**Platform:** LeetCode  
**Problem Number:** 84  
**Difficulty:** Hard

---

# 1. Intuition

We are given an array of non-negative integers representing the heights of bars in a histogram.

We need to find the **largest rectangular area** that can be formed using one or more consecutive bars.

For example:

```text
heights = [2,1,5,6,2,3]
```

The largest rectangle has:

```text
Height = 5
Width = 2
Area = 5 × 2 = 10
```

A brute-force approach would consider every possible rectangle and can take **O(n²)** or more time.

Instead, we use a **Monotonic Stack**.

The stack stores indices of bars in **increasing order of height**.

Whenever the current bar is shorter than the bar at the top of the stack, we know that the taller bar can no longer extend to the right. We then calculate its maximum possible rectangle area.

---

# 2. Approach

## Algorithm

1. Create an empty stack to store bar indices.
2. Initialize:
   ```cpp
   int maxArea = 0;
   ```
3. Traverse the histogram from left to right.
4. For every index `i`, get the current height.
5. If the current height is smaller than the height at the top of the stack:
   - Remove the top index from the stack.
   - Treat that bar as the height of the rectangle.
   - Calculate its possible width.
   - Calculate the area:
     ```cpp
     area = height * width;
     ```
   - Update `maxArea`.
6. Continue until the stack becomes valid again.
7. A final height of `0` is used after the last bar to force all remaining bars in the stack to be processed.
8. Return `maxArea`.

---

## Monotonic Stack

The stack maintains indices whose corresponding heights are in **increasing order**.

For example:

```text
heights = [2,1,5,6]
```

The stack may contain:

```text
height:
1
5
6
```

When a smaller height is encountered, the larger bars are removed and their maximum possible areas are calculated.

---

## Why Do We Store Indices?

We store indices instead of heights because we need to calculate the **width** of the rectangle.

For a popped bar:

```cpp
int height = heights[stack.top()];
```

The index remaining at the top of the stack tells us where the rectangle can start.

The current index `i` tells us where it ends.

Therefore:

```cpp
width = i - stack.top() - 1;
```

If the stack becomes empty:

```cpp
width = i;
```

because the rectangle can extend from index `0` to `i - 1`.

---

## Sentinel Zero

The loop is written as:

```cpp
for (int i = 0; i <= heights.size(); i++)
```

Notice the `<=` instead of `<`.

When:

```cpp
i == heights.size()
```

we use:

```cpp
int currentHeight = 0;
```

This artificial `0` height forces all remaining bars in the stack to be popped and processed.

Without this final `0`, some bars may remain in the stack and their areas would not be calculated.

---

## Example

### Input

```text
heights = [2,1,5,6,2,3]
```

### Dry Run

Initially:

```text
stack = empty
maxArea = 0
```

---

### Index 0

```text
height = 2
```

Stack is empty, so push index `0`.

```text
stack = [0]
```

---

### Index 1

```text
height = 1
```

Current height `1` is smaller than:

```text
heights[0] = 2
```

So index `0` is popped.

```text
height = 2
```

Stack becomes empty, therefore:

```text
width = 1
```

Area:

```text
area = 2 × 1 = 2
```

So:

```text
maxArea = 2
```

Now push index `1`.

```text
stack = [1]
```

---

### Index 2

```text
height = 5
```

Since `5 > 1`, push index `2`.

```text
stack = [1,2]
```

---

### Index 3

```text
height = 6
```

Since `6 > 5`, push index `3`.

```text
stack = [1,2,3]
```

---

### Index 4

```text
height = 2
```

Current height `2` is smaller than height `6`.

Pop index `3`:

```text
height = 6
```

The previous smaller bar is at index `2`.

Therefore:

```text
width = 4 - 2 - 1
      = 1
```

Area:

```text
area = 6 × 1
     = 6
```

So:

```text
maxArea = 6
```

Now height `2` is still smaller than height `5`.

Pop index `2`:

```text
height = 5
```

The previous smaller bar is at index `1`.

Therefore:

```text
width = 4 - 1 - 1
      = 2
```

Area:

```text
area = 5 × 2
     = 10
```

Update:

```text
maxArea = 10
```

Now push index `4`.

```text
stack = [1,4]
```

---

### Index 5

```text
height = 3
```

Since `3 > 2`, push index `5`.

```text
stack = [1,4,5]
```

---

### Final Step — Sentinel Zero

After processing all bars:

```text
i = 6
currentHeight = 0
```

The `0` is smaller than all remaining bars, so they are popped and processed.

The largest area remains:

```text
maxArea = 10
```

Therefore:

```text
Output = 10
```

---

# 3. Code

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> stack;
        int maxArea = 0;

        for (int i = 0; i <= heights.size(); i++) {
            int currentHeight = (i == heights.size()) ? 0 : heights[i];

            while (!stack.empty() &&
                   currentHeight < heights[stack.top()]) {

                int height = heights[stack.top()];
                stack.pop();

                int width;

                if (stack.empty()) {
                    width = i;
                } else {
                    width = i - stack.top() - 1;
                }

                int area = height * width;
                maxArea = max(maxArea, area);
            }

            if (i < heights.size()) {
                stack.push(i);
            }
        }

        return maxArea;
    }
};
```

---

## Complexity Analysis

| **Complexity** | **Value** |
|---|---|
| Time | **O(n)** |
| Space | **O(n)** |

### Why is the time complexity O(n)?

Although there is a `while` loop inside the `for` loop, every index is:

- pushed into the stack at most once
- popped from the stack at most once

Therefore, the total number of stack operations is proportional to `n`.

Hence:

```text
Time = O(n)
```

---

## Key Concepts

- Histogram
- Largest Rectangle
- Stack
- Monotonic Stack
- Increasing Stack
- Index Tracking
- Rectangle Area
- Height × Width
- Previous Smaller Element
- Next Smaller Element
- Sentinel Value
- `stack`
- `push()`
- `pop()`
- `top()`
- `O(n)` Time Complexity
- `O(n)` Space Complexity
