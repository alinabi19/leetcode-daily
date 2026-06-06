# 2574. Left and Right Sum Differences

🔗 LeetCode: https://leetcode.com/problems/left-and-right-sum-differences/

---

# Problem

For every index, calculate:

- Sum of elements on the left side.
- Sum of elements on the right side.

Return an array where:

```text
answer[i] = |leftSum - rightSum|
```

We need the difference for every position efficiently.

---

# Core Idea

Instead of recalculating left and right sums for every index, maintain:

- Total array sum.
- Running left sum.

Right sum can be derived instantly:

```text
rightSum = totalSum - leftSum - nums[i]
```

This avoids nested loops.

---

# Intuition

At any position:

- We already know everything on the left through `leftSum`.
- We can get everything on the right using the total sum.

So we never need to traverse left or right separately.

Think:

```text
Total Sum
= Left + Current + Right
```

```text
Right
= Total - Left - Current
```

---

# Approach

### Step 1

Compute total array sum.

### Step 2

Initialize:

```csharp
leftSum = 0;
```

### Step 3

Traverse the array.

Calculate:

```csharp
rightSum = totalSum - leftSum - nums[i];
```

### Step 4

Store:

```csharp
abs(leftSum - rightSum)
```

### Step 5

Add current element to `leftSum`.

### Step 6

Return the result array.

Brute Force would calculate left and right sums for every index separately, resulting in:

```text
O(n²)
```

This optimized approach does everything in one pass.

---

# Dry Run

### Example

```text
nums = [10,4,8,3]
```

Total Sum:

```text
25
```

### i = 0

```text
left = 0
right = 25 - 0 - 10 = 15
```

```text
ans[0] = |0 - 15| = 15
```

```text
left = 10
```

### i = 1

```text
left = 10
right = 25 - 10 - 4 = 11
```

```text
ans[1] = |10 - 11| = 1
```

```text
left = 14
```

### i = 2

```text
left = 14
right = 25 - 14 - 8 = 3
```

```text
ans[2] = |14 - 3| = 11
```

```text
left = 22
```

### i = 3

```text
left = 22
right = 25 - 22 - 3 = 0
```

```text
ans[3] = |22 - 0| = 22
```

Result:

```text
[15,1,11,22]
```

---

# Pattern Used

## Prefix Sum / Running Sum

### Common signals

- Need sum on the left and right of each index.
- Multiple range sum calculations.
- Want O(n) instead of O(n²).

Whenever you see:

```text
left side sum
right side sum
sum before index
sum after index
```

Think Prefix Sum.

---

# Complexity Analysis

### Time Complexity

```text
O(n)
```

- One pass for total sum.
- One pass to build answer.

### Space Complexity

```text
O(n)
```

Output array of size `n`.

```text
Extra auxiliary space = O(1)
```

---

# Mistakes to Avoid

## 1. Forgetting to exclude current element from right sum

Wrong:

```csharp
right = total - left;
```

Correct:

```csharp
right = total - left - nums[i];
```

---

## 2. Updating leftSum before calculating answer

Calculate the answer first, then update:

```csharp
left += nums[i];
```

---

## 3. Missing absolute difference

Use:

```csharp
Math.Abs(left - right)
```

---

# Interview Tips

Interviewers usually expect the Prefix Sum observation.

Mention that brute force is:

```text
O(n²)
```

Show how right sum can be derived from total sum.

Highlight that no extra prefix array is needed since a running left sum is enough.

---

# C# Solution

```csharp
public class Solution {
    public int[] LeftRightDifference(int[] nums) {
        int sum = 0;
        int n = nums.Length;

        foreach (int num in nums) {
            sum += num;
        }

        int[] ans = new int[n];
        int left = 0;

        for (int i = 0; i < n; i++) {
            int right = sum - left - nums[i];
            ans[i] = Math.Abs(right - left);
            left += nums[i];
        }

        return ans;
    }
}
```
