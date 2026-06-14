# 2130. Maximum Twin Sum of a Linked List

🔗 LeetCode: https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/

---

# Problem

In an even-length linked list, every node has a "twin":

```text
0 ↔ n-1
1 ↔ n-2
2 ↔ n-3
...
```

Twin Sum = value of node + value of its twin.

Return the maximum twin sum among all pairs.

What the question is ACTUALLY asking:

Find the largest sum formed by pairing elements from the beginning and end of the list.

---

# Core Idea

Linked lists don't allow direct indexing.

Convert the linked list into an array first.

Then twin nodes become easy to access:

```text
nums[i]
nums[n-1-i]
```

Check every twin pair and keep the maximum sum.

---

# Intuition

Imagine the linked list as:

```text
5 → 4 → 2 → 1
```

After storing values:

```text
[5,4,2,1]
```

Twin pairs become:

```text
5 + 1
4 + 2
```

Now it's just a simple two-ended array problem.

---

# Approach

### Step 1

Traverse the linked list.

### Step 2

Store all node values in a `List<int>`.

### Step 3

Let `n` be the size of the list.

### Step 4

Iterate from:

```text
0 → n/2 - 1
```

### Step 5

For each index:

Compute:

```text
nums[i] + nums[n-1-i]
```

### Step 6

Maintain the maximum sum.

### Step 7

Return the answer.

---

# Brute Force Thought

Without converting to an array, finding each twin requires traversing repeatedly.

That becomes inefficient.

Array conversion gives O(1) access to both ends.

---

# Dry Run

### Example

```text
head = [5,4,2,1]
```

Array:

```text
nums = [5,4,2,1]
```

```text
n = 4
```

### i = 0

```text
5 + 1 = 6
ans = 6
```

### i = 1

```text
4 + 2 = 6
ans = 6
```

Return:

```text
6
```

---

# Pattern Used

## Linked List → Array Conversion

### Common signals

- Need access from both ends.
- Need random indexing.
- Linked list structure makes direct access difficult.

Ask yourself:

```text
Would this become easier if I had an array?
```

If yes, converting is often a valid interview solution.

---

# Complexity Analysis

### Time Complexity

```text
O(n)
```

- One traversal to build array.
- One traversal for twin sums.

### Space Complexity

```text
O(n)
```

Array/List stores all node values.

---

# Mistakes to Avoid

### 1. Using

Wrong:

```csharp
nums[n - i]
```

Correct:

```csharp
nums[n - 1 - i]
```

---

### 2. Iterating all n elements

Only iterate:

```text
n/2
```

pairs.

---

### 3. Forgetting that the list length is guaranteed to be even

The twin relationship relies on this property.

---

### 4. Not updating the maximum correctly

Always compare the current twin sum with the best answer so far.

---

# Interview Tips

This solution is simple and acceptable in interviews.

After giving this solution, mention an optimization:

### O(1) Extra Space Approach

1. Find middle using Slow/Fast pointers.
2. Reverse second half.
3. Compare first half and reversed second half.

Interviewers often ask for this follow-up.

Always present:

```text
Array Solution → Easy
Reverse Half Solution → Optimal
```

---

# C# Solution

```csharp
public class Solution {
    public int PairSum(ListNode head) {
        List<int> nums = new();

        while (head != null) {
            nums.Add(head.val);
            head = head.next;
        }

        int ans = 0;
        int n = nums.Count;

        for (int i = 0; i < n / 2; i++) {
            ans = Math.Max(ans, nums[i] + nums[n - 1 - i]);
        }

        return ans;
    }
}
```
