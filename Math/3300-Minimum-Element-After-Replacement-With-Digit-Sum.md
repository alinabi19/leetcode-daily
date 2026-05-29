# 3300. Minimum Element After Replacement With Digit Sum

🔗 LeetCode: https://leetcode.com/problems/minimum-element-after-replacement-with-digit-sum/

---

# Problem

We are given an integer array `nums`.

For every element:

- Replace it with the sum of its digits.

After all replacements, return:

> The minimum element in the array.

---

# Core Idea

For each number:

1. Calculate its digit sum
2. Track the minimum digit sum seen so far

---

## Key Observation

We do not need to actually modify the array.

We only need:

```text
minimum digit sum
```

---

# Intuition

## Example

```text
nums = [10,12,13,14]
```

Digit sums:

```text
10 → 1
12 → 3
13 → 4
14 → 5
```

Minimum:

```text
1
```

Instead of creating a new array:

```text
[1,3,4,5]
```

we can directly compute the minimum while iterating.

---

# Approach

## Step 1: Initialize minimum

```csharp
int min = int.MaxValue;
```

---

## Step 2: Calculate digit sum for each number

Use modulo and division:

```csharp
while(x > 0)
{
    sum += x % 10;
    x /= 10;
}
```

---

## Step 3: Update minimum

```csharp
min = Math.Min(min, sum);
```

---

## Step 4: Return answer

```csharp
return min;
```

---

# Dry Run

## Input

```text
nums = [999,19,199]
```

---

## Process 999

```text
9 + 9 + 9 = 27
min = 27
```

---

## Process 19

```text
1 + 9 = 10
min = 10
```

---

## Process 199

```text
1 + 9 + 9 = 19
min = 10
```

---

## Answer

```text
10
```

---

# Pattern Used

## Digit Extraction

---

## Signals for this pattern

- Sum of digits
- Reverse digits
- Count digits
- Digit manipulation

---

# Complexity Analysis

Let:

```text
n = nums.Length
d = maximum digits in a number
```

---

## Time Complexity

```text
O(n × d)
```

### Why?

Each number's digits are processed once.

Since:

```text
nums[i] ≤ 10⁴
```

we have:

```text
d ≤ 5
```

So the solution is practically linear.

---

## Space Complexity

```text
O(1)
```

### Why?

Only a few variables are used.

---

# Mistakes to Avoid

## 1. Forgetting to reset sum

Wrong:

```csharp
int sum = 0;
```

outside the loop.

Each number needs its own digit sum.

---

## 2. Using string conversion unnecessarily

Example:

```csharp
num.ToString()
```

works, but digit extraction is cleaner and more efficient.

---

## 3. Actually modifying the array

Not needed.

We only care about:

```text
minimum digit sum
```

---

# Interview Tips

Interviewers usually expect:

- Basic digit manipulation
- Clean modulo/division logic
- O(1) extra space solution

---

## Core Observation

We can compute each digit sum independently and maintain the minimum value on the fly.

No additional array or data structure is required.

---

# C# Solution

```csharp
public class Solution {

    public int MinElement(int[] nums) {

        int min = int.MaxValue;

        foreach (int num in nums) {

            int x = num;
            int sum = 0;

            while (x > 0) {

                sum += x % 10;
                x /= 10;
            }

            min = Math.Min(min, sum);
        }

        return min;
    }
}
```
