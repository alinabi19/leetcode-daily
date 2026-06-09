# 3689. Maximum Total Subarray Value I

🔗 LeetCode: https://leetcode.com/problems/maximum-total-subarray-value-i/

---

# Problem

We need to choose exactly `k` subarrays.

The value of a subarray is:

```text
max(subarray) - min(subarray)
```

Subarrays can overlap and can even be the same subarray multiple times.

Return the maximum possible total value.

What the question is actually asking:

If you're allowed to pick the best subarray repeatedly, what is the largest total value you can obtain after choosing exactly `k` subarrays?

---

# Core Idea

Since overlapping is allowed and the same subarray can be chosen multiple times, we only need to find:

```text
Maximum possible subarray value
```

The largest possible value comes from a subarray containing:

- Global maximum element
- Global minimum element

So:

```text
Best Value = GlobalMax - GlobalMin
```

Since we can choose the same subarray `k` times:

```text
Answer = (GlobalMax - GlobalMin) × k
```

---

# Intuition

Normally, choosing multiple subarrays creates optimization complexity.

But here:

- Overlapping is allowed.
- Reusing the same subarray is allowed.

That completely changes the problem.

If one subarray gives the highest value, simply select it `k` times.

Therefore:

```text
Find the largest possible value once
Multiply by k
```

---

# Approach

### Step 1

Traverse the array.

### Step 2

Find:

- Minimum element
- Maximum element

### Step 3

Compute:

```text
max - min
```

### Step 4

Multiply by `k`.

### Step 5

Return the result.

No subarray generation is required.

---

# Dry Run

### Example

```text
nums = [1,3,2]
k = 2
```

Find extremes:

```text
min = 1
max = 3
```

Best subarray value:

```text
3 - 1 = 2
```

Choose that subarray twice:

```text
2 + 2 = 4
```

Answer:

```text
4
```

---

# Pattern Used

## Mathematical Observation

### Common signals

- Same choice can be reused multiple times.
- Overlapping is allowed.
- Need maximum score/value repeatedly.

Ask:

```text
Can I reuse the best answer multiple times?
```

If yes, many DP/Greedy-looking problems collapse into a simple observation.

---

# Complexity Analysis

### Time Complexity

```text
O(n)
```

One scan to find min and max.

### Space Complexity

```text
O(1)
```

Only a few variables used.

---

# Mistakes to Avoid

### 1. Thinking distinct subarrays are required

The same subarray can be selected multiple times.

---

### 2. Trying DP or Sliding Window

Neither is needed.

---

### 3. Generating all subarrays

Unnecessary and too expensive.

---

### 4. Forgetting that the same subarray can be selected multiple times

This is the key observation that simplifies the problem.

---

### 5. Using int for the final answer

```text
(max - min) * k
```

may exceed `int` range.

Use:

```csharp
long
```

---

# Interview Tips

This is an observation-based problem.

The trap is overthinking subarrays.

The moment you notice:

```text
overlaps allowed
same subarray allowed multiple times
```

the problem becomes:

```text
Find maximum possible subarray value once.
Multiply by k.
```

Interviewers often use problems like this to test whether you read constraints carefully before designing a complex algorithm.

---

# C# Solution

```csharp
public class Solution {
    public long MaxTotalValue(int[] nums, int k) {
        int min = int.MaxValue;
        int max = int.MinValue;

        foreach (int num in nums) {
            min = Math.Min(min, num);
            max = Math.Max(max, num);
        }

        return (long)(max - min) * k;
    }
}
```
