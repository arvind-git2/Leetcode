# Implement Queue using Stacks

**Platform:** LeetCode  
**Problem Number:** 232  
**Difficulty:** Easy

---

# 1. Intuition

A **Queue** follows the **FIFO (First In, First Out)** principle.

For example:

```text
Queue:
10 → 20 → 30

First element inserted = 10
First element removed = 10
```

A **Stack**, on the other hand, follows **LIFO (Last In, First Out)**.

```text
Stack:
30
20
10
```

The challenge is to implement a Queue using only Stack operations.

To achieve this, we use **two stacks**:

```cpp
stack<int> in, out;
```

- `in` is used to store newly inserted elements.
- `out` is used to provide the correct FIFO order for `pop()` and `peek()`.

Whenever `out` is empty, we transfer all elements from `in` to `out`.

This reverses the order of the elements and makes the oldest element available at the top of `out`.

---

# 2. Approach

## Algorithm

1. Create two stacks:
   ```cpp
   stack<int> in, out;
   ```
2. For `push(x)`, simply push the element into the `in` stack.
3. For `pop()`:
   - Check whether `out` is empty.
   - If it is empty, transfer all elements from `in` to `out`.
   - The top of `out` is now the oldest element.
   - Remove and return it.
4. For `peek()`:
   - Perform the same transfer if `out` is empty.
   - Return the top element of `out` without removing it.
5. For `empty()`:
   - The queue is empty only when both stacks are empty.

---

## Transfer Operation

The `transfer()` function is the main part of the implementation.

```cpp
void transfer() {
    if (out.empty()) {
        while (!in.empty()) {
            out.push(in.top());
            in.pop();
        }
    }
}
```

The transfer is performed **only when `out` is empty**.

Suppose we push:

```text
1, 2, 3
```

The `in` stack becomes:

```text
3
2
1
```

Now `out` is empty, so we transfer:

```text
in → out
```

After transfer:

```text
out:

1
2
3
```

Now `1` is at the top of `out`, which is exactly what we need for FIFO behavior.

---

## Example

### Input

```text
push(1)
push(2)
peek()
pop()
empty()
```

### Step 1 — `push(1)`

```cpp
in.push(1);
```

Stacks:

```text
in:
1

out:
empty
```

Queue:

```text
1
```

---

### Step 2 — `push(2)`

```cpp
in.push(2);
```

Stacks:

```text
in:
2
1

out:
empty
```

Queue:

```text
1 → 2
```

---

### Step 3 — `peek()`

We call:

```cpp
transfer();
```

Since `out` is empty, all elements are transferred.

Before:

```text
in:

2
1
```

After:

```text
out:

1
2
```

Now:

```cpp
return out.top();
```

returns:

```text
1
```

So:

```text
Output = 1
```

---

### Step 4 — `pop()`

Again:

```cpp
transfer();
```

But this time `out` is **not empty**, so no transfer occurs.

The top of `out` is:

```text
1
```

We execute:

```cpp
int val = out.top();
out.pop();
return val;
```

Therefore:

```text
Output = 1
```

Now:

```text
out:

2
```

---

### Step 5 — `empty()`

We execute:

```cpp
return in.empty() && out.empty();
```

Since `out` still contains `2`:

```text
in.empty()  = true
out.empty() = false
```

Therefore:

```text
true && false = false
```

So the queue is not empty.

---

## Important Observation

We **do not transfer elements every time** we perform `pop()` or `peek()`.

We transfer elements only when:

```cpp
out.empty()
```

This is important for efficiency.

For example, after transferring:

```text
out:

1
2
3
```

we can perform:

```text
pop() → 1
pop() → 2
pop() → 3
```

without transferring the elements again.

---

# 3. Code

```cpp
#include <bits/stdc++.h>
using namespace std;

class MyQueue {
private:
    stack<int> in, out;

    void transfer() {
        if (out.empty()) {
            while (!in.empty()) {
                out.push(in.top());
                in.pop();
            }
        }
    }

public:
    MyQueue() {
        
    }
    
    void push(int x) {
        in.push(x);
    }
    
    int pop() {
        transfer();
        int val = out.top();
        out.pop();
        return val;
    }
    
    int peek() {
        transfer();
        return out.top();
    }
    
    bool empty() {
        return in.empty() && out.empty();
    }
};
```

---

# Complexity Analysis

| **Operation** | **Time Complexity** |
|---|---|
| `push()` | **O(1)** |
| `pop()` | **O(1) Amortized** |
| `peek()` | **O(1) Amortized** |
| `empty()` | **O(1)** |

### Space Complexity

```text
O(n)
```

We use two stacks to store up to `n` elements.

---

## Why is `pop()` O(1) Amortized?

At first, transferring all elements from `in` to `out` can take **O(n)** time.

However, each element is transferred from `in` to `out` **only once** before being popped.

Therefore, over a sequence of operations, the average cost per operation is:

```text
O(1) amortized
```

---

## Key Concepts

- Queue
- FIFO (First In, First Out)
- Stack
- LIFO (Last In, First Out)
- Two Stack Technique
- Data Structure Implementation
- `stack`
- Lazy Transfer
- Amortized Complexity
- `push()`
- `pop()`
- `peek()`
- `empty()`
- `O(1)` Amortized Operations
- `O(n)` Space Complexity