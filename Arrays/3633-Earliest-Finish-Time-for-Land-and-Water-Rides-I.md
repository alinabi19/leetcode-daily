# 3633. Earliest Finish Time for Land and Water Rides I

LeetCode: https://leetcode.com/problems/earliest-finish-time-for-land-and-water-rides-i/

---

## Problem

You are given:

### Land Rides

- `landStartTime[i]` = earliest time ride can start
- `landDuration[i]` = duration of ride

### Water Rides

- `waterStartTime[j]` = earliest time ride can start
- `waterDuration[j]` = duration of ride

You must take:

- Exactly one land ride
- Exactly one water ride

You may take them in any order.

Return:

The earliest possible time when both rides are finished.

---

## Core Idea

For every pair:

```text
Land Ride + Water Ride
```

Try both possible orders:

```text
Option 1
Land → Water

Option 2
Water → Land
```

Compute finishing time for both.

Take the minimum.

---

## Intuition

Suppose:

### Land

```text
Start = 2
Duration = 4
```

### Water

```text
Start = 6
Duration = 3
```

### Land First

Land finishes:

```text
2 + 4 = 6
```

Water opens at:

```text
6
```

Can start immediately:

```text
6 → 9
```

Finish:

```text
9
```

### Water First

Water finishes:

```text
6 + 3 = 9
```

Land already opened at:

```text
2
```

Start at:

```text
9
```

Finish:

```text
13
```

Best:

```text
9
```

---

## Approach

### Step 1

Initialize answer:

```csharp
int ans = int.MaxValue;
```

### Step 2

Try every:

```text
Land Ride
×
Water Ride
```

combination.

### Step 3

Calculate:

```text
Land → Water
```

Land finish:

```text
landStart + landDuration
```

Water can start at:

```csharp
Math.Max(landFinish, waterStart)
```

Water finish:

```text
waterStartActual + waterDuration
```

### Step 4

Calculate:

```text
Water → Land
```

Water finish:

```text
waterStart + waterDuration
```

Land can start at:

```csharp
Math.Max(waterFinish, landStart)
```

Land finish:

```text
landStartActual + landDuration
```

### Step 5

Keep minimum finishing time.

---

## Dry Run

Example:

```text
landStartTime = [2]
landDuration = [4]

waterStartTime = [6]
waterDuration = [3]
```

### Land → Water

Land:

```text
2 → 6
```

Water:

```text
6 → 9
```

Finish:

```text
9
```

### Water → Land

Water:

```text
6 → 9
```

Land:

```text
9 → 13
```

Finish:

```text
13
```

Answer:

```text
9
```

---

## Pattern Used

### Brute Force Pair Enumeration

### Signals

- Small constraints
- Need best combination
- Two independent groups
- Try every pair

---

## Complexity Analysis

Let:

```text
n = land rides
m = water rides
```

### Time Complexity

```text
O(n × m)
```

Why?

Every land ride is paired with every water ride.

### Space Complexity

```text
O(1)
```

Only a few variables are used.

---

## Mistakes to Avoid

### 1. Forgetting Both Orders

Wrong:

```text
Only Land → Water
```

Must also check:

```text
Water → Land
```

### 2. Ignoring Waiting Time

If ride isn't open yet:

```csharp
Math.Max(currentTime, startTime)
```

must be used.

### 3. Using Ride Open Time as Start Time

Wrong:

```text
waterStart + duration
```

Need:

```csharp
Math.Max(landFinish, waterStart)
```

### 4. Choosing Earliest Individual Ride

The earliest ride individually may not produce the earliest overall finish.

Always evaluate complete schedules.

---

## Interview Tips

Interviewers usually expect:

- Correct simulation
- Handling waiting times
- Checking both ride orders

Main insight:

```text
For every land-water pair, simulate both possible orders and take the minimum finishing time.
```

Because constraints are small (`≤ 100`), a straightforward `O(n × m)` solution is completely acceptable.

---

## C# Solution

```csharp
public class Solution {
    public int EarliestFinishTime(
        int[] landStartTime,
        int[] landDuration,
        int[] waterStartTime,
        int[] waterDuration) {

        int ans = int.MaxValue;

        for (int i = 0; i < landStartTime.Length; i++) {

            for (int j = 0; j < waterStartTime.Length; j++) {

                // Land -> Water
                int landFinish =
                    landStartTime[i] + landDuration[i];

                int waterStart =
                    Math.Max(landFinish, waterStartTime[j]);

                int finish1 =
                    waterStart + waterDuration[j];

                // Water -> Land
                int waterFinish =
                    waterStartTime[j] + waterDuration[j];

                int landStart =
                    Math.Max(waterFinish, landStartTime[i]);

                int finish2 =
                    landStart + landDuration[i];

                ans = Math.Min(ans,
                      Math.Min(finish1, finish2));
            }
        }

        return ans;
    }
}
```
