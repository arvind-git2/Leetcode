# Odd Even Linked List

**Platform:** LeetCode  
**Problem Number:** 328  
**Difficulty:** Medium

---

# 1. Intuition

We need to rearrange a linked list so that all nodes at **odd positions** come first, followed by all nodes at **even positions**.

The relative order of the nodes must remain the same.

For example:

```text
1 → 2 → 3 → 4 → 5
```

becomes:

```text
1 → 3 → 5 → 2 → 4
```

We can solve this using two pointers:

- `odd` → keeps track of the last odd-position node.
- `even` → keeps track of the last even-position node.
- `evenHead` → stores the beginning of the even-position list.

Instead of creating new nodes, we rearrange the existing `next` pointers. Therefore, the solution uses **O(1) extra space**.

---

# 2. Approach

## Algorithm

1. If the list has `0` or `1` node, return `head`.
2. Set:
   ```cpp
   odd = head
   even = head->next
   evenHead = even
   ```
3. Traverse the list while `even` and `even->next` are not `NULL`.
4. Connect the current odd node to the next odd node:
   ```cpp
   odd->next = even->next;
   ```
5. Move `odd` to the next odd node.
6. Connect the current even node to the next even node:
   ```cpp
   even->next = odd->next;
   ```
7. Move `even` to the next even node.
8. After the loop, connect the end of the odd list to the beginning of the even list:
   ```cpp
   odd->next = evenHead;
   ```
9. Return `head`.

---

## Example

### Input

```text
head = [1,2,3,4,5]
```

### Initial State

```text
Odd List:   1 → 3 → 5
Even List:  2 → 4
```

The original list is:

```text
1 → 2 → 3 → 4 → 5
```

After rearranging:

```text
1 → 3 → 5 → 2 → 4
```

---

### Step-by-Step

| Step | `odd` | `even` | List Structure |
|-----:|------:|-------:|----------------|
| Initial | 1 | 2 | 1 → 2 → 3 → 4 → 5 |
| 1 | 3 | 4 | 1 → 3, 2 → 4 |
| 2 | 5 | NULL | 1 → 3 → 5, 2 → 4 |
| Final | - | - | 1 → 3 → 5 → 2 → 4 |

---

### Input

```text
head = [2,1,3,5,6,4,7]
```

Odd-position nodes:

```text
2 → 3 → 6 → 7
```

Even-position nodes:

```text
1 → 5 → 4
```

Final result:

```text
2 → 3 → 6 → 7 → 1 → 5 → 4
```

---

## Time Complexity

- **Time:** `O(n)`
- **Space:** `O(1)`

Each node is visited only once, and no additional linked list is created.

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

ListNode* oddEvenList(ListNode* head) {
    // If list has 0 or 1 node
    if (head == nullptr || head->next == nullptr)
        return head;

    // Pointers for odd and even nodes
    ListNode* odd = head;
    ListNode* even = head->next;

    // Store the beginning of even list
    ListNode* evenHead = even;

    while (even != nullptr && even->next != nullptr) {

        // Connect odd node to next odd node
        odd->next = even->next;
        odd = odd->next;

        // Connect even node to next even node
        even->next = odd->next;
        even = even->next;
    }

    // Attach even list after odd list
    odd->next = evenHead;

    return head;
}

void printList(ListNode* head) {
    while (head != nullptr) {
        cout << head->val << " ";
        head = head->next;
    }
}

int main() {
    ListNode* head = new ListNode(1);
    head->next = new ListNode(2);
    head->next->next = new ListNode(3);
    head->next->next->next = new ListNode(4);
    head->next->next->next->next = new ListNode(5);

    head = oddEvenList(head);

    printList(head);

    return 0;
}
```

### Output

```text
1 3 5 2 4
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
- Two Pointer Technique
- Pointer Manipulation
- In-place Rearrangement
- Odd/Even Position Tracking
- Single Pass Traversal
- `O(1)` Extra Space
