# 3093. Longest Common Suffix Queries

🔗 LeetCode: https://leetcode.com/problems/longest-common-suffix-queries/

---

# Problem

We are given:

- `wordsContainer`
- `wordsQuery`

For every query string,
we must find the index of the word in `wordsContainer` having:

1. Longest common suffix
2. If tie → shortest length
3. If still tie → smallest index

Return the answer array.

---

# Core Idea

Suffix problems become prefix problems if we:

```text
reverse the strings
```

---

## Example

```text
"abcd" → "dcba"
```

Now:

```text
common suffix
```

becomes:

```text
common prefix
```

This allows us to use:

## Trie

efficiently.

---

# Intuition

Suppose:

```text
wordsContainer = ["abcd","bcd","xbcd"]
query = "cd"
```

---

## Reverse everything

```text
"abcd" → "dcba"
"bcd"  → "dcb"
"xbcd" → "dcbx"
"cd"   → "dc"
```

Now:

```text
common suffix matching
```

turns into:

```text
common prefix traversal
```

Trie becomes perfect for this.

---

# Approach

## Step 1: Reverse all words

### Why?

```text
Suffix matching → Prefix matching
```

---

## Step 2: Build Trie

Each Trie node stores:

```text
Index
```

Meaning:

> Best candidate word index for this suffix path.

---

## Step 3: Update best index

At every node,
choose:

- smaller word length
- if tie → smaller index

This ensures:

> Trie node always stores optimal answer.

---

## Step 4: Insert reversed container words

Character by character.

---

## Step 5: Process queries

Reverse query.

Traverse Trie:

- continue while path exists
- update best answer from node

If traversal breaks:

> Last valid node gives longest suffix match.

---

# Dry Run

## Input

```text
wordsContainer = ["abcd","bcd","xbcd"]
query = "cd"
```

---

## Reverse

```text
"dcba"
"dcb"
"dcbx"
query = "dc"
```

---

## Trie traversal

```text
d → exists
c → exists
```

Longest matched suffix:

```text
"cd"
```

Candidates:

```text
abcd
bcd
xbcd
```

Shortest length:

```text
"bcd"
```

Answer:

```text
1
```

---

# Pattern Used

## Trie + String Reversal

---

## Signals for this pattern

- Prefix/suffix matching
- Multiple string queries
- Fast repeated lookups
- Longest common prefix/suffix

---

# Complexity Analysis

Let:

```text
N = total characters in wordsContainer
Q = total characters in wordsQuery
```

---

## Time Complexity

```text
O(N + Q)
```

### Why?

- Every character inserted once
- Every query traversed once

---

## Space Complexity

```text
O(N)
```

### Why?

Trie stores all characters.

---

# Mistakes to Avoid

## 1. Forgetting reversal

Without reversing:

```text
Trie cannot efficiently solve suffix matching
```

---

## 2. Wrong tie-breaking

Need:

- shortest length
- smallest index

Very important.

---

## 3. Updating only leaf nodes

Best answer must be updated:

```text
at EVERY Trie node
```

because partial suffix matches matter.

---

## 4. Forgetting root update

Root stores:

```text
global shortest word
```

Useful when no suffix matches exist.

---

# Interview Tips

Interviewers usually expect:

- Recognition that suffix → reversed prefix
- Trie optimization
- Proper tie-breaking logic

---

## Core Observation

Reverse strings to convert suffix problems into prefix problems.

This is one of the most important Trie interview tricks.

---

# C# Solution

```csharp
public class Solution {

    class TrieNode {

        public TrieNode[] Next = new TrieNode[26];
        public int Index = -1;
    }

    public int[] StringIndices(string[] wordsContainer, string[] wordsQuery) {

        TrieNode root = new TrieNode();

        int bestGlobal = 0;

        for (int i = 1; i < wordsContainer.Length; i++) {

            if (wordsContainer[i].Length < wordsContainer[bestGlobal].Length)
                bestGlobal = i;
        }

        for (int i = 0; i < wordsContainer.Length; i++) {

            string word = Reverse(wordsContainer[i]);

            TrieNode node = root;

            Update(node, i, wordsContainer);

            foreach (char c in word) {

                int idx = c - 'a';

                if (node.Next[idx] == null)
                    node.Next[idx] = new TrieNode();

                node = node.Next[idx];

                Update(node, i, wordsContainer);
            }
        }

        int[] ans = new int[wordsQuery.Length];

        for (int i = 0; i < wordsQuery.Length; i++) {

            string query = Reverse(wordsQuery[i]);

            TrieNode node = root;

            int best = root.Index;

            foreach (char c in query) {

                int idx = c - 'a';

                if (node.Next[idx] == null)
                    break;

                node = node.Next[idx];

                best = node.Index;
            }

            ans[i] = best;
        }

        return ans;
    }

    void Update(TrieNode node, int index, string[] words) {

        if (node.Index == -1 ||
            words[index].Length < words[node.Index].Length) {

            node.Index = index;
        }
    }

    string Reverse(string s) {

        char[] arr = s.ToCharArray();

        Array.Reverse(arr);

        return new string(arr);
    }
}
```
