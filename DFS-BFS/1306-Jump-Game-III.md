# 1306. Jump Game III

🔗 LeetCode:
https://leetcode.com/problems/jump-game-iii/

## Problem

Given an array `arr` and starting index `start`.

From index `i`:
- Jump forward → `i + arr[i]`
- Jump backward → `i - arr[i]`

Goal:
- Return `true` if we can reach ANY index having value `0`.

---

## Key Observation

This is actually a **Graph Traversal** problem.

- Each index = node
- Possible jumps = edges

Need:
- DFS or BFS
- `visited[]` to avoid infinite loops

---

## Logic

1. Start DFS from `start`
2. If:
   - index out of bounds → return `false`
   - already visited → return `false`
   - value is `0` → return `true`
3. Mark current index as visited
4. Explore:
   - `i + arr[i]`
   - `i - arr[i]`

---

## Pattern

- Graph Traversal
- DFS/BFS
- Reachability Problem
- Implicit Graph

---

## Important Learning

Many array movement problems are secretly graph problems.

Keywords to recognize:
- "can reach"
- "move/jump from index"
- "explore all paths"

→ Think DFS/BFS immediately.

---

## Complexity

- Time: `O(n)`
- Space: `O(n)`

---

## Interview Line

Each index behaves like a graph node, and jumps act like edges. Since cycles are possible, we use DFS with a visited array to avoid revisiting nodes.

---

## Code

```csharp
public class Solution {

    public bool CanReach(int[] arr, int start) {

        bool[] visited = new bool[arr.Length];

        return DFS(arr, start, visited);
    }

    private bool DFS(int[] arr, int i, bool[] visited) {

        if(i < 0 || i >= arr.Length) return false;

        if(visited[i]) return false;

        if(arr[i] == 0) return true;

        visited[i] = true;

        return DFS(arr, i + arr[i], visited) ||
               DFS(arr, i - arr[i], visited);
    }
}
```
