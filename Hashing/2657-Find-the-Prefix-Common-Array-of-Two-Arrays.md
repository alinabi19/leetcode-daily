# 2657. Find the Prefix Common Array of Two Arrays

🔗 LeetCode:
https://leetcode.com/problems/find-the-prefix-common-array-of-two-arrays/

---

# Problem

You are given two permutation arrays:

```text
A
B
```

For every index `i`:

Find how many numbers are common in:

```text
A[0...i]
B[0...i]
```

Return the prefix common array.

Example:

```text
A = [1,3,2,4]
B = [3,1,2,4]
```

Result:

```text
[0,2,3,4]
```

---

# Key Observation

Both arrays are permutations.

A number becomes "common" only after it appears in BOTH arrays.

We can track occurrences using:

```text
freq[]
```

Whenever:

```text
freq[x] == 2
```

that number has now appeared in both arrays.

So:

```text
common++
```

---

# Logic

Initialize:

```text
common = 0
freq[]
```

Loop through arrays together.

At every index:

### Step 1

Add:

```text
A[i]
```

If:

```text
freq[A[i]] == 2
```

increase:

```text
common++
```

---

### Step 2

Add:

```text
B[i]
```

If:

```text
freq[B[i]] == 2
```

increase:

```text
common++
```

---

### Step 3

Store current count:

```text
res[i] = common
```

---

# Pattern

- Frequency Counting
- Prefix Processing
- Simulation
- Array Traversal

---

# Important Learning

When a problem asks:

- "common so far"
- "prefix result"
- "track appearances"

Think about:

```text
frequency array / hashmap
```

instead of recalculating every prefix again and again.

---

# Complexity

- Time: `O(n)`
- Space: `O(n)`

---

# Code

```csharp
public class Solution {
    public int[] FindThePrefixCommonArray(int[] A, int[] B) {
        int n=A.Length;
        int[] res = new int[n];
        int[] freq = new int[n+1];

        int common=0;
        for(int i=0; i<n; i++){
            freq[A[i]]++;
            if(freq[A[i]]==2) common++;

            freq[B[i]]++;
            if(freq[B[i]]==2) common++;

            res[i] = common;
        }
        return res;
    }
}
```

---

# Interview Line

Instead of checking common elements for every prefix separately, we track frequencies while traversing both arrays together. A number becomes common exactly when its frequency reaches `2`.
