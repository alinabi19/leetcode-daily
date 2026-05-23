# 1752. Check if Array Is Sorted and Rotated

🔗 LeetCode: https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/

---

# Problem

We are given an array.

We need to check whether the array was:

- Originally sorted in non-decreasing order
- Then rotated some number of times

Return:

- `true` if possible
- `false` otherwise

---

## Example

```text
[3,4,5,1,2]
```

Originally:

```text
[1,2,3,4,5]
```

Rotated.

The question is actually asking:

> Can this array become fully sorted if we rotate it properly?

---

# Core Idea

In a sorted array:

```text
nums[i] <= nums[i+1]
```

This condition breaks only at the rotation point.

---

## Example

```text
[3,4,5,1,2]
```

Only one place breaks sorting:

```text
5 > 1
```

---

## Key Observation

> A valid sorted + rotated array can have at most ONE decreasing point.

If more than one exists:

```text
Not possible
```

---

# Intuition

Think of the array as circular.

---

## Example

```text
[3,4,5,1,2]
```

Comparisons:

```text
3 <= 4 ✔
4 <= 5 ✔
5 <= 1 ✘
1 <= 2 ✔
2 <= 3 ✔
```

Only one violation.

So valid.

---

## Example

```text
[2,1,3,4]
```

Comparisons:

```text
2 > 1 ✘
1 <= 3 ✔
3 <= 4 ✔
4 > 2 ✘
```

Two violations.

Not valid.

---

# Approach

## Step 1

Traverse entire array.

---

## Step 2

Compare current element with next element.

Since array is circular:

```csharp
(i + 1) % n
```

handles wrap-around comparison.

---

## Step 3

Count how many times:

```text
nums[i] > nums[i+1]
```

---

## Step 4

If count exceeds `1`:

```csharp
return false;
```

Else:

```csharp
return true;
```

---

# Dry Run

## Input

```text
nums = [3,4,5,1,2]
```

---

## Comparisons

```text
3 <= 4 ✔
4 <= 5 ✔
5 > 1 ✘  count = 1
1 <= 2 ✔
2 <= 3 ✔
```

---

## Final

```text
count = 1
```

Valid rotated sorted array.

---

## Answer

```text
true
```

---

# Pattern Used

## Circular Array Observation

---

## Signals for this pattern

- Rotated array
- Circular traversal
- Only one break point allowed
- Compare neighboring elements

---

# Complexity Analysis

## Time Complexity

```text
O(n)
```

### Why?

Single traversal of array.

---

## Space Complexity

```text
O(1)
```

### Why?

Only counter variable used.

---

# Mistakes to Avoid

## 1. Forgetting circular comparison

Need:

```csharp
(i + 1) % n
```

Otherwise last element won't compare with first.

---

## 2. Using >= instead of >

Array can contain duplicates.

Correct:

```csharp
nums[i] > nums[(i+1)%n]
```

NOT:

```csharp
>=
```

---

## 3. Thinking rotation must happen

Rotation by `0` is allowed.

Example:

```text
[1,2,3]
```

is valid.

---

## 4. More than one decreasing point

That immediately means invalid.

---

# Interview Tips

Interviewers usually expect:

- Recognition of rotation property
- Circular traversal understanding
- Observation-based optimization

---

## Most Important Insight

> Sorted rotated arrays have at most one inversion point.

This is a very common interview observation pattern.

---

# C# Solution

```csharp
public class Solution {

    public bool Check(int[] nums) {

        int n = nums.Length;
        int count = 0;

        for (int i = 0; i < n; i++) {

            if (nums[i] > nums[(i + 1) % n])
                count++;
        }

        return count <= 1;
    }
}
```

---

# Final Insight

Instead of trying every possible rotation:

- Observe the number of decreasing points
- A valid rotated sorted array can only break ordering once

That reduces the problem to a simple:

```text
Single pass observation problem
```
