# 1345. Jump Game IV

🔗 LeetCode:
https://leetcode.com/problems/jump-game-iv/

## Problem

Given an array `arr`, you start at index `0`.

In one step, you can jump to:
- `i + 1`
- `i - 1`
- Any index `j` where:
  ```text
  arr[i] == arr[j]
  ```

Goal:
- Return minimum number of jumps needed to reach last index.

---

## Key Observation

This is a **Shortest Path / BFS** problem.

Each index = graph node

Possible jumps = edges:
- left
- right
- same value indices

Since we need:
```text
minimum jumps
```

→ Think BFS immediately.

---

## Important Optimization

Store:
```text
value -> list of indices
```

using HashMap / Dictionary.

Example:

```text
100 -> [0,4]
23 -> [5,6,7]
```

This avoids scanning whole array repeatedly.

---

## VERY IMPORTANT TRICK

After processing:
```text
arr[i]
```

clear its list:

```text
map[arr[i]].Clear()
```

Why?

To avoid revisiting same-value indices again and again.

Without this:
```text
O(n²)
```

---

## Logic

1. Create:
   - Queue for BFS
   - Visited array
   - Dictionary:
     ```text
     value -> indices
     ```

2. Start BFS from index `0`

3. From current index:
   - jump left
   - jump right
   - jump to same-value indices

4. Mark visited indices

5. After processing same-value group:
   ```text
   Clear hashmap list
   ```

6. First time reaching last index = minimum jumps

---

## Pattern

- BFS
- Shortest Path
- Graph Traversal
- HashMap Optimization
- Implicit Graph

---

## Important Learning

Keywords to recognize:
- "minimum jumps"
- "minimum operations"
- "shortest path"

→ Think BFS immediately.

Optimization learning:
```text
Avoid repeated work
```

The hardest part is realizing:
```text
same-value indices should only be processed once
```

---

## Complexity

- Time: `O(n)`
- Space: `O(n)`

Without optimization:
```text
O(n²)
```

---

## Interview Line

Each index behaves like a graph node. Since we need minimum jumps, we use BFS for shortest path traversal. A hashmap stores same-value indices for fast jumps, and clearing processed groups avoids repeated traversal.

---

## Code

```csharp
public class Solution {

    public int MinJumps(int[] arr) {

        int n = arr.Length;

        if (n == 1)
            return 0;

        // value -> indices
        Dictionary<int, List<int>> map = new Dictionary<int, List<int>>();

        for (int i = 0; i < n; i++) {

            if (!map.ContainsKey(arr[i])) {
                map[arr[i]] = new List<int>();
            }

            map[arr[i]].Add(i);
        }

        Queue<int> queue = new Queue<int>();
        bool[] visited = new bool[n];

        queue.Enqueue(0);
        visited[0] = true;

        int steps = 0;

        while (queue.Count > 0) {

            int size = queue.Count;

            for (int s = 0; s < size; s++) {

                int i = queue.Dequeue();

                // reached last index
                if (i == n - 1)
                    return steps;

                List<int> neighbors = new List<int>();

                // same value indices
                neighbors.AddRange(map[arr[i]]);

                // adjacent indices
                neighbors.Add(i - 1);
                neighbors.Add(i + 1);

                foreach (int next in neighbors) {

                    if (next >= 0 &&
                        next < n &&
                        !visited[next]) {

                        visited[next] = true;
                        queue.Enqueue(next);
                    }
                }

                // optimization
                map[arr[i]].Clear();
            }

            steps++;
        }

        return -1;
    }
}
```
