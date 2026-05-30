# 3161. Block Placement Queries

🔗 LeetCode: https://leetcode.com/problems/block-placement-queries/

---

# Problem

We have an infinite number line starting from `0`.

There are two types of queries:

## Type 1

```text
[1, x]
```

Place an obstacle at position `x`.

---

## Type 2

```text
[2, x, sz]
```

Check whether a block of size `sz` can fit completely inside:

```text
[0, x]
```

without intersecting any obstacle.

Return answers for all Type 2 queries.

---

# Core Idea

The challenge is:

> Dynamically maintain the largest empty gap between obstacles.

A brute force approach would repeatedly scan all obstacles for every query, which is too slow.

The key idea is:

## Process Queries in Reverse

Instead of:

```text
Adding obstacles
```

we process backwards and:

```text
Remove obstacles
```

Removing obstacles is much easier because gaps only merge.

---

# Intuition

Suppose obstacles are:

```text
0 --- 2 --- 7 --- 10
```

Gaps:

```text
2
5
3
```

Now remove obstacle `7`:

```text
0 --- 2 ----------- 10
```

New merged gap:

```text
8
```

Notice:

> Removing obstacles only increases gaps.

This makes reverse processing very convenient.

---

# Why Reverse Processing?

## Forward

Adding obstacle:

```text
split gap
```

Hard to maintain efficiently.

---

## Backward

Removing obstacle:

```text
merge gaps
```

Much easier.

This is a classic offline-query trick.

---

# Data Structures Used

## 1. Treap

Stores active obstacle positions.

Supports:

- Previous obstacle
- Next obstacle
- Insert
- Remove

all in:

```text
O(log n)
```

---

## 2. Fenwick Tree (BIT)

Stores:

```text
maximum gap information
```

Allows fast queries:

```text
largest gap before position x
```

in:

```text
O(log n)
```

---

# Approach

## Step 1

Collect all obstacle positions from Type 1 queries.

---

## Step 2

Insert every obstacle into Treap.

Now we know the final state after all queries.

---

## Step 3

Build initial gaps.

Example:

```text
0, 2, 7, 10
```

Store:

```text
2
5
3
```

inside Fenwick Tree.

---

## Step 4

Process queries from right to left.

---

## Type 2 Query

Find:

```text
largest gap before x
```

using Fenwick Tree.

Also compute:

```text
tail gap = x - previousObstacle
```

Answer:

```csharp
Math.Max(bestGap, tailGap) >= sz
```

---

## Type 1 Query

Reverse operation:

```text
Remove obstacle
```

Find:

```text
left obstacle
right obstacle
```

Merge both gaps:

```text
right - left
```

Update Fenwick Tree.

Remove obstacle from Treap.

---

# Dry Run

## Input

```text
[1,2]
[2,3,3]
[2,3,1]
[2,2,2]
```

---

## Final obstacle configuration

```text
0 --- 2
```

Gap:

```text
2
```

---

## Query

```text
[2,2,2]
```

Largest available space:

```text
2
```

Need:

```text
2
```

Answer:

```text
true
```

---

## Query

```text
[2,3,3]
```

Largest available space:

```text
2
```

Need:

```text
3
```

Answer:

```text
false
```

---

# Pattern Used

## Offline Query Processing

### Signals

- Queries depend on previous updates
- Updates are hard forward
- Easier to undo than apply
- Dynamic interval maintenance

---

## Ordered Set + Gap Tracking

### Signals

- Previous / Next element lookup
- Dynamic insert/remove
- Largest interval queries

---

# Complexity Analysis

Let:

```text
n = number of queries
```

---

## Time Complexity

Building structures:

```text
O(n log n)
```

Processing queries:

```text
O(n log n)
```

Overall:

```text
O(n log n)
```

---

## Space Complexity

Treap:

```text
O(n)
```

Fenwick Tree:

```text
O(n)
```

Overall:

```text
O(n)
```

---

# Mistakes to Avoid

## 1. Solving Online

Trying to process obstacle additions directly becomes very complicated.

Reverse processing is the intended optimization.

---

## 2. Forgetting Obstacle at 0

The solution inserts:

```csharp
treap.Insert(0);
```

This acts as the permanent left boundary.

Without it many gap calculations fail.

---

## 3. Wrong Gap Merge

When removing obstacle:

```text
left --- x --- right
```

New gap becomes:

```text
right - left
```

Not:

```text
right - x
```

or

```text
x - left
```

---

## 4. Ignoring Tail Gap

For query:

```text
[0, x]
```

The last segment:

```text
x - previousObstacle
```

must also be considered.

The solution computes:

```csharp
int tailGap = x - pre;
```

---

# Interview Tips

This is a difficult Hard problem.

Interviewers are usually testing:

- Offline query processing
- Ordered set data structures
- Interval maintenance
- Reverse-thinking optimization

---

## Core Observation

Instead of adding obstacles, process queries backwards and remove obstacles.

Once that observation is made, the problem becomes:

```text
Treap + Fenwick Tree + Offline Queries
```

which reduces the complexity to:

```text
O(n log n)
```

---

# C# Solution

```csharp
public class Solution {
    class Fenwick{
        private readonly int[] bit;

        public Fenwick(int n){
            bit = new int[n + 2];
        }

        public void Update(int idx, int val){
            int n = bit.Length - 1;

            while (idx <= n){
                if (val > bit[idx])
                    bit[idx] = val;

                idx += idx & -idx;
            }
        }

        public int Query(int idx){
            int res = 0;

            while (idx > 0){
                if (bit[idx] > res)
                    res = bit[idx];

                idx -= idx & -idx;
            }

            return res;
        }
    }

    class Node{
        public int Key;
        public int Priority;
        public Node Left;
        public Node Right;

        public Node(int key, int priority){
            Key = key;
            Priority = priority;
        }
    }

    class Treap{
        private Node root;
        private readonly Random rnd = new Random(1);

        private Node RotateRight(Node y){
            Node x = y.Left;
            y.Left = x.Right;
            x.Right = y;
            return x;
        }

        private Node RotateLeft(Node x){
            Node y = x.Right;
            x.Right = y.Left;
            y.Left = x;
            return y;
        }

        private Node Insert(Node node, int key){
            if (node == null)
                return new Node(key, rnd.Next());

            if (key < node.Key){
                node.Left = Insert(node.Left, key);

                if (node.Left.Priority > node.Priority)
                    node = RotateRight(node);
            }
            else if (key > node.Key){
                node.Right = Insert(node.Right, key);

                if (node.Right.Priority > node.Priority)
                    node = RotateLeft(node);
            }

            return node;
        }

        public void Insert(int key){
            root = Insert(root, key);
        }

        private Node Remove(Node node, int key){
            if (node == null)
                return null;

            if (key < node.Key) node.Left = Remove(node.Left, key);
            else if (key > node.Key) node.Right = Remove(node.Right, key);
            else{
                if (node.Left == null)
                    return node.Right;

                if (node.Right == null)
                    return node.Left;

                if (node.Left.Priority > node.Right.Priority){
                    node = RotateRight(node);
                    node.Right = Remove(node.Right, key);
                }
                else{
                    node = RotateLeft(node);
                    node.Left = Remove(node.Left, key);
                }
            }

            return node;
        }

        public void Remove(int key){
            root = Remove(root, key);
        }

        public int Prev(int x){
            Node cur = root;
            int ans = -1;

            while (cur != null){
                if (cur.Key <= x){
                    ans = cur.Key;
                    cur = cur.Right;
                }
                else cur = cur.Left;

            }

            return ans;
        }

        public int Next(int x){
            Node cur = root;
            int ans = -1;

            while (cur != null){
                if (cur.Key >= x){
                    ans = cur.Key;
                    cur = cur.Left;
                }
                else cur = cur.Right;
            }

            return ans;
        }
    }

    public IList<bool> GetResults(int[][] queries){
        int maxX = 0;
        int type2Count = 0;

        foreach (var q in queries){
            maxX = Math.Max(maxX, q[1]);

            if (q[0] == 2)
                type2Count++;
        }

        Fenwick bit = new Fenwick(maxX + 5);

        List<int> allObstacles = new List<int>();

        foreach (var q in queries){
            if (q[0] == 1)
                allObstacles.Add(q[1]);
        }

        allObstacles.Sort();

        Treap treap = new Treap();
        treap.Insert(0);

        int prev = 0;

        foreach (int pos in allObstacles){
            treap.Insert(pos);
            bit.Update(pos + 1, pos - prev);
            prev = pos;
        }

        bool[] ans = new bool[type2Count];
        int answerIdx = type2Count - 1;

        for (int i = queries.Length - 1; i >= 0; i--){
            var q = queries[i];

            if (q[0] == 2){
                int x = q[1];
                int sz = q[2];

                int pre = treap.Prev(x);

                int bestGap = bit.Query(pre + 1);
                int tailGap = x - pre;

                ans[answerIdx--] = Math.Max(bestGap, tailGap) >= sz;
            }
            else{
                int x = q[1];

                int left = treap.Prev(x - 1);
                int right = treap.Next(x + 1);

                if (right != -1) bit.Update(right + 1, right - left);

                treap.Remove(x);
            }
        }

        return ans;
    }
}
```
