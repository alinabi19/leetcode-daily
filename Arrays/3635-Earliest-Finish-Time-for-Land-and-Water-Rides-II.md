# 3635. Earliest Finish Time for Land and Water Rides II

LeetCode: https://leetcode.com/problems/earliest-finish-time-for-land-and-water-rides-ii/

---

## Problem

You are given two categories of rides:

### Land Rides

- `landStartTime[i]` = earliest time ride can start
- `landDuration[i]` = duration of ride

### Water Rides

- `waterStartTime[j]` = earliest time ride can start
- `waterDuration[j]` = duration of ride

A tourist must take:

- Exactly one land ride
- Exactly one water ride

The rides can be taken in any order.

Return:

The earliest possible time when both rides are completed.

---

## Core Idea

The previous version (3633) allowed brute force because constraints were small.

This version has:

```text
n, m ≤ 50,000
```

Brute force:

```text
O(n × m)
```

is impossible.

Key observation:

If we take:

```text
Land → Water
```

then for a land ride finishing at:

```text
finish
```

we need the best water ride that minimizes:

```text
max(waterStart, finish) + waterDuration
```

Instead of checking every water ride repeatedly, we precompute the best possible answer.

---

## Intuition

Suppose:

### Water Rides

| Start | Duration |
|---------|---------|
| 1 | 10 |
| 6 | 3 |
| 20 | 2 |

If land finishes at:

```text
5
```

Then:

```text
Ride 1 → 11
Ride 2 → 9
Ride 3 → 22
```

Best:

```text
9
```

If land finishes at:

```text
15
```

Then:

```text
Ride 1 → 25
Ride 2 → 18
Ride 3 → 22
```

Best:

```text
18
```

Instead of recomputing every time:

```text
We precompute optimal choices.
```

---

## Approach

Your solution uses a helper:

```csharp
solve(start1, duration1, start2, duration2)
```

which computes:

```text
Category1 → Category2
```

### Step 1

Compute finish times of first category.

```text
finish1 =
start1[i] + duration1[i]
```

Store:

```text
minimum finish time
```

among all rides.

### Step 2

For every ride in second category:

Compute:

```csharp
Math.Max(start2[i], finish1)
+ duration2[i]
```

This represents:

```text
Finish first ride
↓
Take second ride
↓
Final completion time
```

### Step 3

Take minimum over all possibilities.

### Step 4

Compute both orders:

```text
Land → Water
Water → Land
```

Return:

```csharp
Math.Min(...)
```

---

## Dry Run

Example:

```text
landStart = [2,8]
landDuration = [4,1]

waterStart = [6]
waterDuration = [3]
```

### Land → Water

Land finishes:

```text
2+4 = 6
8+1 = 9
```

Best land finish:

```text
6
```

Water:

```text
max(6,6)+3
= 9
```

Answer:

```text
9
```

### Water → Land

Water finish:

```text
6+3 = 9
```

Land:

```text
max(2,9)+4 = 13
max(8,9)+1 = 10
```

Best:

```text
10
```

Overall:

```text
min(9,10)
= 9
```

---

## Pattern Used

### Precomputation + Greedy Optimization

### Signals

- Huge constraints
- Need minimum answer
- Repeated pair evaluations
- Brute force too expensive

### Scheduling Optimization

### Signals

- Start times
- Finish times
- Waiting allowed
- Earliest completion required

---

## Complexity Analysis

Let:

```text
n = land rides
m = water rides
```

### Time Complexity

Each solve:

```text
O(n + m)
```

Two solves:

```text
O(n + m)
```

### Space Complexity

```text
O(1)
```

Only a few variables used.

---

## Mistakes to Avoid

### 1. Using Brute Force

Wrong:

```text
Check every land-water pair
```

Complexity:

```text
O(n × m)
```

Too slow for 50k constraints.

### 2. Forgetting Both Orders

Need:

```text
Land → Water
```

and

```text
Water → Land
```

### 3. Ignoring Waiting Time

Must use:

```csharp
Math.Max(startTime, finishTime)
```

because rides may not be open yet.

### 4. Mixing Start Time and Finish Time

Carefully distinguish:

```text
Ride opening time
```

vs

```text
Ride completion time
```

---

## Interview Tips

Interviewers are testing whether you can:

- Eliminate O(n²)
- Identify reusable computations
- Optimize scheduling problems

Main insight:

```text
Once the first ride category is fixed, only the earliest possible completion matters.
```

Precomputing that minimum allows the entire problem to be solved in linear time.

---

## C# Solution

```csharp
public class Solution {

    private int solve(
        int[] start1,
        int[] duration1,
        int[] start2,
        int[] duration2) {

        int finish1 = int.MaxValue;

        for (int i = 0; i < start1.Length; i++) {

            finish1 = Math.Min(
                finish1,
                start1[i] + duration1[i]);
        }

        int finish2 = int.MaxValue;

        for (int i = 0; i < start2.Length; i++) {

            finish2 = Math.Min(
                finish2,
                Math.Max(start2[i], finish1)
                + duration2[i]);
        }

        return finish2;
    }

    public int EarliestFinishTime(
        int[] landStartTime,
        int[] landDuration,
        int[] waterStartTime,
        int[] waterDuration) {

        int landWater =
            solve(
                landStartTime,
                landDuration,
                waterStartTime,
                waterDuration);

        int waterLand =
            solve(
                waterStartTime,
                waterDuration,
                landStartTime,
                landDuration);

        return Math.Min(
            landWater,
            waterLand);
    }
}
```

----
**Tags:** Arrays, Greedy, Precomputation, Scheduling, Optimization
