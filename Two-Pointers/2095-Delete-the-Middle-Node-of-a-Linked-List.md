# 2095. Delete the Middle Node of a Linked List

🔗 LeetCode: https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/

---

# Problem

Given a linked list, delete its middle node.

The middle node is:

```text
⌊ n / 2 ⌋
```

using 0-based indexing.

Return the head of the modified list.

What the question is ACTUALLY asking:

Find the middle node efficiently and remove it without using extra space.

---

# Core Idea

Use the classic Slow and Fast Pointer technique.

When:

```text
slow moves 1 step
fast moves 2 steps
```

then when fast reaches the end:

```text
slow = middle node
```

Keep a `prev` pointer so we can remove the middle node.

---

# Intuition

Instead of counting all nodes first:

```text
1 → 3 → 4 → 7 → 1 → 2 → 6
```

Let:

```text
slow = moves 1 step
fast = moves 2 steps
```

Since fast moves twice as quickly, by the time it reaches the end:

```text
slow lands exactly at the middle
```

Then simply skip that node:

```text
prev.next = slow.next
```

---

# Approach

### Step 1

Handle edge case:

If list contains only one node, return `null`.

### Step 2

Initialize:

```csharp
prev = null
slow = head
fast = head
```

### Step 3

Move pointers:

```csharp
while(fast != null && fast.next != null)
```

- Store current `slow` in `prev`
- Move `slow` one step
- Move `fast` two steps

### Step 4

After loop:

```text
slow points to middle node
prev points to node before middle
```

### Step 5

Delete middle:

```csharp
prev.next = slow.next
```

### Step 6

Return head.

---

# Dry Run

### Example

```text
1 → 3 → 4 → 7 → 1 → 2 → 6
```

Initial:

```text
prev = null
slow = 1
fast = 1
```

### Iteration 1

```text
prev = 1
slow = 3
fast = 4
```

### Iteration 2

```text
prev = 3
slow = 4
fast = 1
```

### Iteration 3

```text
prev = 4
slow = 7
fast = 6
```

Loop ends.

Middle:

```text
slow = 7
```

Delete:

```text
prev.next = slow.next
```

Result:

```text
1 → 3 → 4 → 1 → 2 → 6
```

---

# Pattern Used

## Slow & Fast Pointers

### Common signals

- Find middle node.
- Delete middle node.
- Split linked list into halves.
- Detect cycle.

Whenever you see:

```text
middle of linked list
```

think:

```text
Slow + Fast Pointer
```

---

# Complexity Analysis

### Time Complexity

```text
O(n)
```

Single traversal of the list.

### Space Complexity

```text
O(1)
```

Only a few pointers used.

---

# Mistakes to Avoid

### 1. Forgetting the single-node case

```text
[1]
```

should return:

```text
null
```

---

### 2. Not maintaining prev

Trying to delete `slow` directly without access to the previous node.

---

### 3. Using counting approach when slow/fast pointers solve it in one pass

The two-pointer solution is cleaner and more efficient.

---

# Interview Tips

This is a classic Slow/Fast Pointer problem.

Interviewers usually expect the one-pass solution.

Mention that a two-pass solution exists:

1. Count nodes.
2. Find middle.
3. Delete.

Then explain why Slow/Fast Pointer is better:

```text
One traversal
O(1) space
Cleaner implementation
```

---

# C# Solution

```csharp
public class Solution {
    public ListNode DeleteMiddle(ListNode head) {
        if (head == null || head.next == null)
            return null;

        ListNode prev = null;
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }

        prev.next = slow.next;

        return head;
    }
}
```
