# 33. Search in Rotated Sorted Array

🔗 LeetCode: https://leetcode.com/problems/search-in-rotated-sorted-array/

---

# Problem

We are given a sorted array that has been rotated at some pivot.

We need to find the index of `target`.

If target does not exist, return `-1`.

---

## Example

```text
[4,5,6,7,0,1,2]
```

Originally:

```text
[0,1,2,4,5,6,7]
```

Rotated at some point.

The question is actually asking:

> How can we still use Binary Search even though sorting is broken by rotation?

---

# Core Idea

Even after rotation:

> One half of the array is ALWAYS sorted.

At every step:

1. Find middle
2. Identify which side is sorted
3. Check if target belongs inside that sorted half
4. Discard the other half

This keeps Binary Search alive.

---

# Intuition

Normally Binary Search works because:

```text
Left side < Right side
```

Rotation breaks full ordering.

But not completely.

---

## Example

```text
[4,5,6,7,0,1,2]
```

If:

```text
mid = 7
```

Left side:

```text
[4,5,6,7]
```

is sorted.

Right side:

```text
[0,1,2]
```

is also sorted separately.

At least one side remains ordered.

So we:

- Detect sorted half
- Decide whether target lies there
- Move accordingly

This is the main trick.

---

# Approach

## Step 1: Initialize Binary Search pointers

```csharp
int i = 0;
int j = nums.Length - 1;
```

---

## Step 2: Find middle

```csharp
int m = i + (j - i) / 2;
```

---

## Step 3: If target found, return index

```csharp
if(nums[m] == target)
    return m;
```

---

## Step 4: Check which half is sorted

---

## Left half sorted

```csharp
if(nums[i] <= nums[m])
```

Now check:

Does target lie between `nums[i]` and `nums[m]`?

If yes:

- Search left

Else:

- Search right

---

## Right half sorted

Else case means:

```text
Right half is sorted
```

Now check:

Does target lie between `nums[m]` and `nums[j]`?

Move accordingly.

---

# Dry Run

## Input

```text
nums = [4,5,6,7,0,1,2]
target = 0
```

---

## Iteration 1

```text
i = 0
j = 6
m = 3
nums[m] = 7
```

Left half:

```text
[4,5,6,7]
```

sorted.

Target `0` is NOT inside:

```text
4 → 7
```

So:

```text
Search right
i = m + 1
```

---

## Iteration 2

```text
i = 4
j = 6
m = 5
nums[m] = 1
```

Left half:

```text
[0,1]
```

sorted.

Target `0` lies inside.

So:

```text
j = m - 1
```

---

## Iteration 3

```text
m = 4
nums[m] = 0
```

Found target.

---

## Answer

```text
4
```

---

# Pattern Used

## Modified Binary Search


## Signals for this pattern

- Sorted rotated array
- `O(log n)` requirement
- Search problem
- “One half is sorted”
- Pivot rotation involved

---

# Complexity Analysis

## Time Complexity

```text
O(log n)
```

### Why?

We discard half of array every iteration.

---

## Space Complexity

```text
O(1)
```

### Why?

No extra data structures used.

---

# Mistakes to Avoid

## 1. Forgetting sorted-half detection

Main logic:

```csharp
if(nums[i] <= nums[m])
```

Without this, Binary Search fails.

---

## 2. Wrong boundary comparisons

Need:

```csharp
target >= nums[i]
target < nums[m]
```

Be careful with inclusive/exclusive checks.

---

## 3. Infinite loop

Always move:

```csharp
i = m + 1
```

or

```csharp
j = m - 1
```

Never:

```csharp
i = m
j = m
```

---

## 4. Overflow in mid calculation

Correct:

```csharp
int m = i + (j - i) / 2;
```

Safer than:

```csharp
(i + j) / 2
```

---

# Interview Tips

Interviewers usually expect:

- Recognition that one side remains sorted
- Binary Search adaptation skills
- Careful boundary handling

---

## Most Important Insight

> Rotation breaks global sorting, but not local half sorting.

---

## Classic Follow-up

### What if duplicates exist?

Problem:

```text
Search in Rotated Sorted Array II
```

That version becomes trickier because sorted-half detection becomes ambiguous.

---

# C# Solution

```csharp
public class Solution {

    public int Search(int[] nums, int target) {

        int i = 0;
        int j = nums.Length - 1;

        while (i <= j) {

            int m = i + (j - i) / 2;

            if (nums[m] == target)
                return m;

            // Left half is sorted
            if (nums[i] <= nums[m]) {

                if (target >= nums[i] && target < nums[m])
                    j = m - 1;
                else
                    i = m + 1;
            }

            // Right half is sorted
            else {

                if (target > nums[m] && target <= nums[j])
                    i = m + 1;
                else
                    j = m - 1;
            }
        }

        return -1;
    }
}
```
