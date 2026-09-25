# Palindrome Linked List

**Platform:** LeetCode  
**Problem Number:** 234  
**Difficulty:** Easy

---

# 1. Intuition

We need to determine whether a linked list is a **palindrome**.

A linked list is a palindrome if it reads the same from left to right and right to left.

For example:

```text
1 → 2 → 2 → 1
```

is a palindrome because it is the same in both directions.

But:

```text
1 → 2 → 3
```

is not a palindrome.

Since a singly linked list cannot be traversed backwards directly, we can solve the problem efficiently using three techniques:

- **Slow and Fast Pointers** → Find the middle of the linked list.
- **Reverse Linked List** → Reverse the second half.
- **Two Pointers** → Compare the first half with the reversed second half.

We rearrange the existing nodes instead of creating another data structure, so the solution uses **O(1) extra space**.

---

# 2. Approach

## Algorithm

1. If the list has `0` or `1` node, return `true`.
2. Use two pointers:
   ```cpp
   slow = head
   fast = head
   ```
3. Move:
   - `slow` one step at a time.
   - `fast` two steps at a time.
4. When `fast` reaches the end, `slow` will be around the middle of the list.
5. If the list has an odd number of nodes, skip the middle node:
   ```cpp
   if (fast != nullptr)
       slow = slow->next;
   ```
6. Reverse the second half of the linked list.
7. Compare the first half with the reversed second half.
8. If any pair of values is different, return `false`.
9. If all values match, return `true`.

---

## Example

### Input

```text
head = [1,2,2,1]
```

The linked list is:

```text
1 → 2 → 2 → 1
```

### Find the Middle

Using slow and fast pointers:

```text
1 → 2 → 2 → 1
    ↑
   slow
```

The second half starts from:

```text
2 → 1
```

---

### Reverse the Second Half

Before reversal:

```text
2 → 1
```

After reversal:

```text
1 → 2
```

Now we have:

```text
First Half:   1 → 2
Second Half:  1 → 2
```

---

### Compare Both Halves

```text
1 == 1  ✓
2 == 2  ✓
```

All values match.

Therefore:

```text
true
```

---

## Step-by-Step

| Step | `slow` | `fast` | Action |
|-----:|-------:|-------:|--------|
| Initial | 1 | 1 | Start traversal |
| 1 | 2 | 2 | Move slow by 1, fast by 2 |
| 2 | 2 | NULL | Middle found |
| 3 | - | - | Reverse second half |
| 4 | - | - | Compare both halves |
| Final | - | - | `true` |

---

### Input

```text
head = [1,2,3,2,1]
```

The linked list is:

```text
1 → 2 → 3 → 2 → 1
```

The middle node is:

```text
1 → 2 → 3 → 2 → 1
        ↑
      middle
```

Since the list has an odd number of nodes, we skip the middle node `3`.

Second half:

```text
2 → 1
```

After reversing:

```text
1 → 2
```

Now compare:

```text
First Half:   1 → 2
Second Half:  1 → 2
```

Both halves match.

Therefore:

```text
true
```

---

### Non-Palindrome Example

Input:

```text
head = [1,2,3]
```

Linked list:

```text
1 → 2 → 3
```

Skip the middle node:

```text
1 → 2 → 3
    ↑
   middle
```

Second half:

```text
3
```

Compare:

```text
1 != 3
```

Therefore:

```text
false
```

---

## Time Complexity

- **Time:** `O(n)`
- **Space:** `O(1)`

The list is traversed a constant number of times, and only a few pointer variables are used.

---

# 3. Code

```cpp
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;

    ListNode(int x) {
        val = x;
        next = nullptr;
    }
};

// Reverse a linked list
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;

    while (curr != nullptr) {
        ListNode* nextNode = curr->next;

        curr->next = prev;
        prev = curr;
        curr = nextNode;
    }

    return prev;
}

bool isPalindrome(ListNode* head) {

    // If list has 0 or 1 node
    if (head == nullptr || head->next == nullptr)
        return true;

    // Find the middle using slow and fast pointers
    ListNode* slow = head;
    ListNode* fast = head;

    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;
    }

    // If the list has odd number of nodes,
    // skip the middle node
    if (fast != nullptr) {
        slow = slow->next;
    }

    // Reverse the second half
    ListNode* secondHalf = reverseList(slow);

    // Compare first half and reversed second half
    ListNode* firstHalf = head;

    while (secondHalf != nullptr) {

        if (firstHalf->val != secondHalf->val)
            return false;

        firstHalf = firstHalf->next;
        secondHalf = secondHalf->next;
    }

    return true;
}

void printResult(bool result) {
    if (result)
        cout << "true";
    else
        cout << "false";
}

int main() {

    ListNode* head = new ListNode(1);
    head->next = new ListNode(2);
    head->next->next = new ListNode(2);
    head->next->next->next = new ListNode(1);

    bool result = isPalindrome(head);

    printResult(result);

    return 0;
}
```

### Output

```text
true
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(1)** |

---

## Key Concepts

- Linked List
- Slow and Fast Pointer
- Finding the Middle Node
- Linked List Reversal
- Two Pointer Technique
- Palindrome Checking
- In-place Processing
- `O(1)` Extra Space
