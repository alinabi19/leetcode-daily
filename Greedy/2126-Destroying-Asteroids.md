# 2126. Destroying Asteroids

LeetCode: https://leetcode.com/problems/destroying-asteroids/

---

## Problem

You are given:

- Initial planet mass `mass`
- Array `asteroids`

Rules:

- If planet mass ≥ asteroid mass, the asteroid is destroyed
- Planet gains that asteroid's mass
- Otherwise, the planet is destroyed

We can choose the collision order.

Return:

- `true` if all asteroids can be destroyed
- `false` otherwise

---

## Example

```text
mass = 10
asteroids = [3,9,19,5,21]
```

One possible order:

```text
10 -> 13 -> 18 -> 27 -> 46 -> 67
```

All asteroids are destroyed.

Answer:

```text
true
```

---
## Core Idea

To maximize our chances of survival:

Always destroy the smallest asteroid first.

Why?

Because smaller asteroids are easier to absorb and increase our mass, helping us destroy larger asteroids later.

This is a classic:

`Greedy Strategy`

---

## Intuition

Suppose:

```text
mass = 10
asteroids = [3,9,19,5,21]
```

If we process:

```text
19 first
```

We immediately fail:

```text
10 < 19
```

Instead sort:

```text
[3,5,9,19,21]
```

Process:

```text
10 → 13
13 → 18
18 → 27
27 → 46
46 → 67
```

Everything gets destroyed.

The smallest asteroids act like "free power-ups".

---

## Approach

### Step 1:

Sort asteroids in ascending order.

```csharp
Array.Sort(asteroids);
```

### Step 2:

Store current mass.

Use:

```csharp
long curMass = mass;
```

because mass can become very large.

### Step 3:

Traverse sorted asteroids.

If:

```text
curMass < asteroid
```

we cannot destroy it.

Return:

```text
false
```

### Step 4:

Otherwise absorb asteroid.

```csharp
curMass += asteroid;
```

### Step 5:

If all asteroids are processed:

```text
return true
```

---

## Dry Run

Example:

```text
mass = 5
asteroids = [4,9,23,4]
```

Sort:

```text
[4,4,9,23]
```

Process 4:

```text
5 → 9
```

Process 4:

```text
9 → 13
```

Process 9:

```text
13 → 22
```

Process 23:

```text
22 < 23
```

Cannot destroy.

Answer:

```text
false
```

---

## Pattern Used

### Greedy + Sorting

### Signals for this pattern:

- We can choose any order
- Local optimal choice helps future decisions
- Smaller elements unlock larger elements

## Complexity Analysis

Let:

```text
n = asteroids.Length
```

### Time Complexity:

```text
O(n log n)
```

Why?

- Sorting dominates
- Traversal is O(n)

### Space Complexity:

```text
O(1)
```

Ignoring sorting implementation space.

---

## Mistakes to Avoid

### 1. Not sorting

Wrong:

```text
Process asteroids in given order
```

The order is arbitrary and often suboptimal.

### 2. Using int instead of long

Mass keeps increasing:

```text
10 + 100000 + 100000 + ...
```

Can overflow int.

Correct:

```csharp
long curMass
```

### 3. Thinking DP is needed

The greedy choice is always optimal:

```text
Destroy smallest available asteroid first
```

No need for DP or backtracking.

---

## Interview Tips

Interviewers usually expect:

- Recognition that order can be chosen
- Greedy reasoning
- Sorting before processing

Main insight:

```text
Every asteroid destroyed increases your future power.
```

Therefore:

```text
Take the easiest wins first.
```

This naturally leads to sorting in ascending order.

---

## C# Solution

```csharp
public class Solution {

    public bool AsteroidsDestroyed(int mass, int[] asteroids) {

        Array.Sort(asteroids);

        long curMass = mass;

        foreach (int asteroid in asteroids) {

            if (curMass < asteroid)
                return false;

            curMass += asteroid;
        }

        return true;
    }
}
```
