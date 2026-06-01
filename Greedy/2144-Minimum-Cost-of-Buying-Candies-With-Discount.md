# 2144. Minimum Cost of Buying Candies With Discount

LeetCode: https://leetcode.com/problems/minimum-cost-of-buying-candies-with-discount/

---

## Problem

A candy shop has a special offer:

For every 2 candies you buy, you can get 1 additional candy for free.

Condition:

The free candy's cost must be less than or equal to the cheaper of the two purchased candies.

Return:

The minimum amount needed to buy all candies.

---

## Core Idea

To maximize the discount:

Make the most expensive candies become the "paid" candies and get the next most expensive eligible candy for free.

After sorting in descending order:

```text
Most expensive
↓
9, 7, 6, 5, 2, 2
```

For every group of 3 candies:

```text
Pay → Pay → Free
```

The third candy should be free.

---

## Intuition

Suppose:

```text
cost = [6,5,7,9,2,2]
```

Sort descending:

```text
[9,7,6,5,2,2]
```

Group them:

```text
9,7,(6 free)
5,2,(2 free)
```

Total paid:

```text
9 + 7 + 5 + 2 = 23
```

The greedy idea is:

```text
Always use expensive candies to unlock the largest possible free candy.
```

---

## Approach

### Step 1

Sort the array.

```csharp
Array.Sort(cost);
```

### Step 2

Reverse it to get descending order.

```csharp
Array.Reverse(cost);
```

### Step 3

Traverse the sorted array.

Every third candy:

```text
index = 2, 5, 8, ...
```

becomes free.

### Step 4

Add only paid candies.

Your condition:

```csharp
if ((i + 1) % 3 != 0)
```

means:

```text
1st candy → pay
2nd candy → pay
3rd candy → free
```

### Step 5

Return total cost.

---

## Dry Run

Example:

```text
cost = [6,5,7,9,2,2]
```

### Sort Descending

```text
[9,7,6,5,2,2]
```

### Traverse

```text
9 → pay
7 → pay
6 → free

5 → pay
2 → pay
2 → free
```

### Total

```text
9 + 7 + 5 + 2
= 23
```

Answer:

```text
23
```

---

## Pattern Used

### Greedy + Sorting

### Signals for this pattern

- Can choose items in any order
- Need maximum discount / minimum cost
- Local optimal choices lead to global optimum

## Complexity Analysis

Let:

```text
n = cost.Length
```

### Time Complexity

```text
O(n log n)
```

Why?

- Sorting dominates
- Traversal is O(n)

### Space Complexity

```text
O(1)
```

Ignoring sorting implementation space.

---

## Mistakes to Avoid

### 1. Sorting Ascending

Wrong:

```text
[2,2,5,6,7,9]
```

You would waste discounts on cheap candies.

Always sort descending.

### 2. Freeing the Wrong Candy

For every 3 candies:

```text
Pay
Pay
Free
```

The third candy becomes free.

Not the first or second.

### 3. Forgetting Remaining Candies

Example:

```text
[9,7,6,5]
```

Only:

```text
6
```

is free.

The last candy:

```text
5
```

must still be paid.

### 4. Overcomplicating with DP

This is purely a greedy sorting problem.

No DP needed.

---

## Interview Tips

Interviewers usually expect:

- Recognition that order can be rearranged
- Greedy reasoning
- Sorting before grouping

Main insight:

```text
To maximize savings, make the largest possible candy in each group of three become the free candy.
```

Sorting descending naturally achieves this.

---

## C# Solution

```csharp
public class Solution {
    public int MinimumCost(int[] cost) {

        Array.Sort(cost);
        Array.Reverse(cost);

        int minCost = 0;

        for (int i = 0; i < cost.Length; i++) {

            if ((i + 1) % 3 != 0)
                minCost += cost[i];
        }

        return minCost;
    }
}
```
