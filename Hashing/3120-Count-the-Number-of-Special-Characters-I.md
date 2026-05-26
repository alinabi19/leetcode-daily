# 3120. Count the Number of Special Characters I

🔗 LeetCode: https://leetcode.com/problems/count-the-number-of-special-characters-i/

---

# Problem

We are given a string `word`.

A character is called **special** if:

- its lowercase version exists
- AND its uppercase version also exists

We need to return:

> Number of special characters.

---

## Example

```text
word = "aaAbcBC"
```

Special characters:

```text
a, b, c
```

Answer:

```text
3
```

---

# Core Idea

We only care whether:

- lowercase exists
- uppercase exists

---

## Key Observation

> Presence matters, frequency does NOT.

So:

- Store all characters in a `HashSet`
- Check every lowercase letter:
  - does lowercase exist?
  - does uppercase exist?

If yes:

```text
count++
```

---

# Intuition

Instead of counting characters repeatedly,
we can use:

```text
HashSet
```

because it gives:

```text
O(1)
```

lookup.

For every letter:

```text
a → check A
b → check B
c → check C
```

Simple membership checking solves the problem efficiently.

---

# Approach

## Step 1: Store all characters in a HashSet

```csharp
HashSet<char> s = new HashSet<char>(word);
```

Now we can quickly check character existence.

---

## Step 2: Loop from `'a'` to `'z'`

---

## Step 3: For each character, check both cases

Check:

```csharp
s.Contains(c)
```

AND:

```csharp
s.Contains(char.ToUpper(c))
```

---

## Step 4: If both exist

```csharp
count++;
```

---

## Step 5: Return final count

---

# Dry Run

## Input

```text
word = "aaAbcBC"
```

---

## HashSet

```text
{ a, A, b, c, B, C }
```

---

## Check letters

```text
a + A ✔
b + B ✔
c + C ✔
d + D ✘
...
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

## Hashing / Presence Checking

---

## Signals for this pattern

- Fast existence lookup
- Presence matters more than frequency
- Pair checking
- Character matching

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

- Building `HashSet` takes `O(n)`
- Looping 26 letters is constant

---

## Space Complexity

```text
O(n)
```

### Why?

`HashSet` stores characters.

---

# Mistakes to Avoid

## 1. Using nested loops

Checking every pair manually becomes unnecessary.

Use `HashSet` for fast lookup.

---

## 2. Forgetting uppercase conversion

Need:

```csharp
char.ToUpper(c)
```

---

## 3. Counting duplicates multiple times

We only count:

```text
unique letters
```

NOT occurrences.

---

## 4. Using string.Contains repeatedly

That can increase complexity.

`HashSet` lookup is faster.

---

# Interview Tips

Interviewers usually expect:

- Use of `HashSet` for membership checking
- Understanding that frequency doesn't matter
- Clean character handling

---

## Core Observation

We only care whether both lowercase and uppercase forms exist.

The number of occurrences does not matter.

---

# C# Solution

```csharp
public class Solution {

    public int NumberOfSpecialChars(string word) {

        HashSet<char> s = new HashSet<char>(word);

        int count = 0;

        for (char c = 'a'; c <= 'z'; c++) {

            if (s.Contains(c) && s.Contains(char.ToUpper(c)))
                count++;
        }

        return count;
    }
}
```
