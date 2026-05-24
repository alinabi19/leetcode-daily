# 1340. Jump Game V

🔗 LeetCode: https://leetcode.com/problems/jump-game-v/

---

# Problem

We are given:

- an array `arr`
- a maximum jump distance `d`

From index `i`, we can jump:

- left or right
- at most `d` steps

BUT only if:

```text
arr[i] > arr[j]
```

Also:

Every element between `i` and `j`
must be smaller than `arr[i]`.

We need to return:

> Maximum number of indices we can visit starting from any index.

---

# Core Idea

This is a:

## DFS + Memoization problem

---

## Key Observation

From every index, we want the best possible path length.

So:

- Try all valid jumps
- Recursively compute best answer
- Cache result to avoid recomputation

---

# Intuition

Think of every index as a node.

From one index:

- we can jump only to smaller values
- jumps stop if we hit a bigger/equal value

---

## Example

```text
10 → 8 → 6 → 4
```

This forms a DAG-like structure:

```text
always decreasing
```

So:

- DFS works naturally
- Memoization prevents repeated DFS calls

---

# Approach

## Step 1: Create DFS function

```csharp
DFS(index)
```

Meaning:

> Maximum indices visitable starting from index.

---

## Step 2: Memoization

If already computed:

```csharp
if(memo[index] != 0)
    return memo[index];
```

---

## Step 3: Try jumping LEFT

From:

```text
index - 1
```

to:

```text
index - d
```

### Conditions

- inside bounds
- smaller value

If blocked by bigger/equal element:

```csharp
break;
```

because further jumps become impossible.

---

## Step 4: Try jumping RIGHT

Same logic.

---

## Step 5: Store best result

```csharp
memo[index] = best;
```

---

## Step 6: Run DFS from every index

Because:

> Best starting point is unknown.

---

# Dry Run

## Input

```text
arr = [6,4,14,6,8,13,9,7,10,6,12]
d = 2
```

---

## Start from

```text
index = 10
value = 12
```

Possible jumps:

```text
10 → 8 (10)
8 → 6 (9)
6 → 7 (7)
```

Total visited:

```text
4
```

DFS computes:

> Best path from every index.

Memo stores results so repeated paths are avoided.

---

# Pattern Used

## DFS + Memoization (Top Down DP)

---

## Signals for this pattern

- “maximum path”
- recursive exploration
- repeated subproblems
- choices from every index
- DAG-like traversal

---

# Complexity Analysis

Let:

```text
n = arr.Length
```

---

## Time Complexity

```text
O(n × d)
```

### Why?

- Every index explored once due to memoization
- Each DFS checks at most `d` positions left/right

---

## Space Complexity

```text
O(n)
```

### Why?

- Memoization array
- Recursive call stack

---

# Mistakes to Avoid

## 1. Forgetting break condition

Important:

```csharp
if(arr[i] >= arr[index]) break;
```

Because:

> Bigger/equal element blocks further jumps.

---

## 2. Missing memoization

Without memoization:

```text
Exponential recursion
```

TLE likely.

---

## 3. Using visited array unnecessarily

No cycles exist because:

```text
we only jump to smaller values
```

---

## 4. Forgetting current index count

Minimum answer from any index:

```text
1
```

(the index itself)

---

# Interview Tips

Interviewers usually expect:

- Recognition of DFS + DP pattern
- Memoization optimization
- Correct handling of blocking condition

---

## Core Observation

Larger/equal elements stop exploration in that direction.

That is the key rule controlling valid jumps.

---

# C# Solution

```csharp
public class Solution {

    private int[] arr;
    private int d;
    private int[] memo;

    public int MaxJumps(int[] arr, int d) {

        this.arr = arr;
        this.d = d;

        int n = arr.Length;

        memo = new int[n];

        int answer = 1;

        for (int i = 0; i < n; i++) {

            answer = Math.Max(answer, DFS(i));
        }

        return answer;
    }

    private int DFS(int index) {

        if (memo[index] != 0)
            return memo[index];

        int best = 1;

        // Left jumps
        for (int i = index - 1; i >= Math.Max(0, index - d); i--) {

            if (arr[i] >= arr[index])
                break;

            best = Math.Max(best, 1 + DFS(i));
        }

        // Right jumps
        for (int i = index + 1; i <= Math.Min(arr.Length - 1, index + d); i++) {

            if (arr[i] >= arr[index])
                break;

            best = Math.Max(best, 1 + DFS(i));
        }

        return memo[index] = best;
    }
}
```
