# 1871. Jump Game VII

🔗 LeetCode: https://leetcode.com/problems/jump-game-vii/

---

# Problem

We are given:

- a binary string `s`
- `minJump`
- `maxJump`

We start at index:

```text
0
```

We can jump from index `i` to index `j` if:

```text
i + minJump <= j <= i + maxJump
```

AND:

```text
s[j] == '0'
```

We need to determine:

> Can we reach the last index?

---

# Core Idea

This is a:

## Dynamic Programming + Sliding Window problem

---

## Key Observation

For every index, we only need to know:

> Is there ANY reachable index inside the valid jump range?

Instead of checking the whole range every time,
we maintain:

```text
how many reachable positions currently exist
```

using a sliding window.

---

# Intuition

## Brute Force

For every index:

- check all previous positions within jump range

That becomes:

```text
O(n²)
```

Too slow for:

```text
n = 10^5
```

---

## Optimization Idea

Instead of repeatedly scanning ranges:

- maintain a running count of reachable indices

This turns:

```text
range checking
```

into:

```text
O(1)
```

per index.

---

# Approach

## DP Meaning

```csharp
dp[i]
```

means:

> Can we reach index `i`?

---

## Step 1: Start position is reachable

```csharp
dp[0] = true;
```

---

## Step 2: Maintain sliding window count

```csharp
reachable
```

stores:

> How many reachable indices currently exist
inside valid jump range.

---

## Step 3: Expand window

When:

```text
i >= minJump
```

Index:

```text
i - minJump
```

enters valid range.

If reachable:

```csharp
reachable++;
```

---

## Step 4: Shrink window

When:

```text
i > maxJump
```

Index:

```text
i - maxJump - 1
```

leaves valid range.

If reachable:

```csharp
reachable--;
```

---

## Step 5: Determine current reachability

Current index is reachable if:

- At least one reachable index exists in window
- Current character is `'0'`

```csharp
dp[i] = reachable > 0 && s[i] == '0';
```

---

# Dry Run

## Input

```text
s = "011010"
minJump = 2
maxJump = 3
```

---

## Initial

```text
dp[0] = true
reachable = 0
```

---

## i = 2

Index:

```text
0
```

enters window.

Since:

```text
dp[0] = true
```

```text
reachable = 1
```

Current:

```text
s[2] = '1'
```

So:

```text
dp[2] = false
```

---

## i = 3

Window still contains reachable index.

```text
s[3] = '0'
reachable > 0
```

So:

```text
dp[3] = true
```

Eventually:

```text
dp[n-1] = true
```

---

## Answer

```text
true
```

---

# Pattern Used

## Dynamic Programming + Sliding Window

---

## Signals for this pattern

- Reachability problem
- Range-based transitions
- “Can jump within interval”
- Need optimization from `O(n²)`

---

# Complexity Analysis

## Time Complexity

```text
O(n)
```

### Why?

- Every index processed once
- Sliding window updates are `O(1)`

---

## Space Complexity

```text
O(n)
```

### Why?

DP array used.

---

# Mistakes to Avoid

## 1. Brute force range scanning

Wrong:

```text
checking every previous index
```

Leads to:

```text
O(n²)
```

TLE.

---

## 2. Wrong sliding window boundaries

Carefully handle:

```text
i - minJump
i - maxJump - 1
```

Very common bug area.

---

## 3. Forgetting current character check

Need:

```csharp
s[i] == '0'
```

Cannot land on `'1'`.

---

## 4. Confusing reachable count

`reachable` stores:

> Count of reachable indices inside current valid jump range.

NOT total reachable positions.

---

# Interview Tips

Interviewers usually expect:

- Recognition of DP transition optimization
- Sliding window usage
- Ability to reduce `O(n²) → O(n)`

---

## Core Observation

We only care whether at least one reachable index exists inside the current jump window.

That allows range transitions to be optimized efficiently.

---

# C# Solution

```csharp
public class Solution {

    public bool CanReach(string s, int minJump, int maxJump) {

        int n = s.Length;

        bool[] dp = new bool[n];
        dp[0] = true;

        int reachable = 0;

        for (int i = 1; i < n; i++) {

            // Add new index entering window
            if (i >= minJump && dp[i - minJump]) {

                reachable++;
            }

            // Remove index leaving window
            if (i > maxJump && dp[i - maxJump - 1]) {

                reachable--;
            }

            dp[i] = reachable > 0 && s[i] == '0';
        }

        return dp[n - 1];
    }
}
```
