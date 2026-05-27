# 3121. Count the Number of Special Characters II

🔗 LeetCode: https://leetcode.com/problems/count-the-number-of-special-characters-ii/

---

# Problem

We are given a string `word`.

A character is called **special** if:

- It appears in both lowercase and uppercase
- ALL lowercase occurrences appear BEFORE the FIRST uppercase occurrence

We need to return:

> Number of special characters.

---

# Core Idea

For every letter:

- track the last lowercase index
- track the first uppercase index

A character is special if:

```text
last lowercase index < first uppercase index
```

---

## Key Observation

> Order matters now, not just presence.

---

# Intuition

## Example

```text
"aaAbcBC"
```

For:

```text
a
```

Lowercase positions:

```text
0,1
```

Uppercase position:

```text
2
```

Condition:

```text
1 < 2 ✔
```

So valid.

---

## Invalid Case

```text
"AbBCab"
```

For:

```text
b
```

Uppercase:

```text
2
```

Lowercase:

```text
4
```

Condition:

```text
4 < 2 ✘
```

Invalid because lowercase appeared AFTER uppercase.

---

# Approach

## Step 1: Create two arrays

```csharp
lower[26]
upper[26]
```

- `lower[i]` stores last lowercase occurrence
- `upper[i]` stores first uppercase occurrence

---

## Step 2: Initialize arrays

```csharp
lower = -1
upper = int.MaxValue
```

### Why?

Helps identify missing characters easily.

---

## Step 3: Traverse string

If lowercase:

```csharp
lower[c - 'a'] = i;
```

Store latest lowercase index.

If uppercase:

```csharp
upper[c - 'A'] = Math.Min(upper[c - 'A'], i);
```

Store earliest uppercase index.

---

## Step 4: Validate characters

Loop through all 26 letters.

A character is special if:

```csharp
lower[i] != -1
&& upper[i] != int.MaxValue
&& lower[i] < upper[i]
```

Then:

```csharp
count++;
```

---

# Dry Run

## Input

```text
word = "aaAbcBC"
```

---

## Track indices

```text
a → lower = 1, upper = 2
b → lower = 3, upper = 5
c → lower = 4, upper = 6
```

---

## Validation

```text
1 < 2 ✔
3 < 5 ✔
4 < 6 ✔
```

---

## Count

```text
3
```

---

## Answer

```text
3
```

---

# Pattern Used

## Index Tracking / Character State Tracking

---

## Signals for this pattern

- Order matters
- Need earliest/latest occurrence
- Character position tracking
- Compare relative ordering

---

# Complexity Analysis

## Time Complexity

```text
O(n + 26)
```

Simplified:

```text
O(n)
```

### Why?

- One traversal of string
- One traversal of 26 letters

---

## Space Complexity

```text
O(26)
```

Simplified:

```text
O(1)
```

### Why?

Fixed-size arrays.

---

# Mistakes to Avoid

## 1. Only checking existence

This problem also requires:

```text
correct ordering
```

Not just lowercase + uppercase presence.

---

## 2. Using first lowercase instead of last lowercase

Need:

```text
ALL lowercase letters before uppercase
```

So:

```text
last lowercase index
```

matters.

---

## 3. Forgetting earliest uppercase

Need:

```text
first uppercase occurrence
```

So use:

```csharp
Math.Min(...)
```

---

## 4. Incorrect initialization

Important:

```csharp
lower = -1
upper = int.MaxValue
```

Used to detect absent characters safely.

---

# Interview Tips

Interviewers usually expect:

- Recognition that order matters
- Earliest/latest occurrence tracking
- Efficient `O(n)` solution

---

## Core Observation

Compare:

```text
last lowercase occurrence
vs
first uppercase occurrence
```

This converts a complicated ordering problem into simple index comparison.

---

# C# Solution

```csharp
public class Solution {

    public int NumberOfSpecialChars(string word) {

        int[] lower = new int[26];
        int[] upper = new int[26];

        Array.Fill(lower, -1);
        Array.Fill(upper, int.MaxValue);

        for (int i = 0; i < word.Length; i++) {

            char c = word[i];

            if (char.IsLower(c))
                lower[c - 'a'] = i;

            else
                upper[c - 'A'] = Math.Min(upper[c - 'A'], i);
        }

        int count = 0;

        for (int i = 0; i < 26; i++) {

            if (lower[i] != -1 &&
                upper[i] != int.MaxValue &&
                lower[i] < upper[i]) {

                count++;
            }
        }

        return count;
    }
}
```
