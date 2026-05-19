# 2540. Minimum Common Value

## Problem

Given two sorted arrays:

```text
nums1
nums2
```

Return the smallest value present in BOTH arrays.

If no common value exists:

```text
return -1
```

---

## Key Observation

Both arrays are already sorted.

This means:
- smaller value can never become equal later
- we can safely move the smaller pointer forward

So we use:

```text
Two Pointers
```

instead of extra space like HashSet.

---

## Logic

Start:

```text
i = 0
j = 0
```

Compare:

### Case 1

```text
nums1[i] < nums2[j]
```

Move:

```text
i++
```

because smaller value cannot match later.

---

### Case 2

```text
nums1[i] > nums2[j]
```

Move:

```text
j++
```

---

### Case 3

```text
nums1[i] == nums2[j]
```

Found smallest common value.

Return:

```text
nums1[i]
```

---

Continue until one pointer goes out of bounds.

If no match found:

```text
return -1
```

---

## Pattern

- Two Pointers
- Sorted Arrays
- Merge Technique

---

## Important Learning

Whenever:
- arrays are sorted
- comparison between two arrays is needed

Think:

```text
Two Pointers
```

instead of:
- nested loops
- HashSet

because sorted order gives optimization for free.

---

## Complexity

- Time: `O(n + m)`
- Space: `O(1)`

---

## Code

```csharp
public class Solution {

    public int GetCommon(int[] nums1, int[] nums2) {

        int i = 0;
        int j = 0;

        while (i < nums1.Length && j < nums2.Length) {

            if (nums1[i] < nums2[j]) {
                i++;
            }
            else if (nums1[i] > nums2[j]) {
                j++;
            }
            else {
                return nums1[i];
            }
        }

        return -1;
    }
}
```

---

## Interview Line

Since both arrays are sorted, we can efficiently solve this using two pointers. At every step, we move the pointer having the smaller value because it can never become a valid answer later.
