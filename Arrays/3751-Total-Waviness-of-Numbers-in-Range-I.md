# 3751. Total Waviness of Numbers in Range I

🔗 LeetCode: https://leetcode.com/problems/total-waviness-of-numbers-in-range-i/

---

## Problem

You are given two integers:

```text
num1
num2
```

representing the inclusive range:

```text
[num1, num2]
```

For each number:

- A digit is a peak if it is strictly greater than both neighbors.
- A digit is a valley if it is strictly smaller than both neighbors.
- First and last digits can never be peaks or valleys.
- Numbers with fewer than 3 digits have waviness = 0.

Return:

```text
Total waviness of all numbers in the range.
```

---

## Core Idea

For every number:

1. Convert it to a string.
2. Check every middle digit.
3. Count:
   - Peaks
   - Valleys
4. Add all counts to the final answer.

A digit contributes to waviness if:

```text
left < current > right
```

or

```text
left > current < right
```

---

## Intuition

Example:

```text
120
```

Middle digit:

```text
2
```

Neighbors:

```text
1 and 0
```

Since:

```text
2 > 1
2 > 0
```

it is a:

```text
Peak
```

Waviness:

```text
1
```

Example:

```text
202
```

Middle digit:

```text
0
```

Neighbors:

```text
2 and 2
```

Since:

```text
0 < 2
0 < 2
```

it is a:

```text
Valley
```

Waviness:

```text
1
```

---

## Approach

### Step 1

Initialize:

```csharp
int ans = 0;
```

### Step 2

Iterate through every number:

```csharp
for(int n = num1; n <= num2; n++)
```

### Step 3

Convert number into string.

```csharp
string s = n.ToString();
```

### Step 4

Check every middle digit.

```csharp
for(int i = 1; i < s.Length - 1; i++)
```

### Step 5

If digit forms a peak:

```text
left < current > right
```

count it.

### Step 6

If digit forms a valley:

```text
left > current < right
```

count it.

### Step 7

Return total count.

---

## Dry Run

Example:

```text
num1 = 198
num2 = 202
```

### 198

```text
1 9 8
```

Middle digit:

```text
9
```

Peak:

```text
1 < 9 > 8
```

Waviness:

```text
1
```

### 199

```text
1 9 9
```

Not peak.

Not valley.

Waviness:

```text
0
```

### 200

```text
2 0 0
```

Not peak.

Not valley.

Waviness:

```text
0
```

### 201

```text
2 0 1
```

Valley:

```text
2 > 0 < 1
```

Waviness:

```text
1
```

### 202

```text
2 0 2
```

Valley:

```text
2 > 0 < 2
```

Waviness:

```text
1
```

Total:

```text
1 + 0 + 0 + 1 + 1 = 3
```

---

## Pattern Used

### Simulation / Digit Traversal

#### Signals

- Process every number in a range
- Analyze digits individually
- Local digit comparison
- Peak/Valley detection

---

## Complexity Analysis

Let:

```text
R = num2 - num1 + 1
D = maximum digits per number
```

### Time Complexity

```text
O(R × D)
```

Why?

- Every number is processed once.
- Every digit is checked once.

### Space Complexity

```text
O(D)
```

For the string representation.

---

## Mistakes to Avoid

### 1. Checking First Digit

Invalid:

```text
i = 0
```

First digit has no left neighbor.

### 2. Checking Last Digit

Invalid:

```text
i = s.Length - 1
```

Last digit has no right neighbor.

### 3. Using >= or <=

Peak requires:

```text
strictly greater
```

Valley requires:

```text
strictly smaller
```

Use:

```text
>
<
```

not:

```text
>=
<=
```

### 4. Forgetting Numbers With Less Than 3 Digits

Example:

```text
7
42
99
```

Waviness:

```text
0
```

because they cannot have a middle digit.

---

## Interview Tips

Interviewers usually expect:

- Careful peak/valley identification
- Proper handling of edge digits
- Clean digit traversal

Main insight:

```text
Waviness is simply the count of local extrema (peaks + valleys) among the middle digits.
```

This turns the problem into a straightforward digit simulation.

---

## C# Solution

```csharp
public class Solution {
    public int TotalWaviness(int num1, int num2) {
        int ans = 0;

        for (int n = num1; n <= num2; n++) {

            string s = n.ToString();

            for (int i = 1; i < s.Length - 1; i++) {

                if ((s[i] > s[i - 1] && s[i] > s[i + 1]) ||
                    (s[i] < s[i - 1] && s[i] < s[i + 1])) {

                    ans++;
                }
            }
        }

        return ans;
    }
}
```
