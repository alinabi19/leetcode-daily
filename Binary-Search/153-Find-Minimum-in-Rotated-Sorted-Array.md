# 153. Find Minimum in Rotated Sorted Array

🔗 LeetCode:
https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/

## Problem

Given a sorted array rotated some number of times.

Goal:
- Find the minimum element in `O(log n)` time.

Example:

```text
[4,5,6,7,0,1,2]
```

Minimum = `0`

---

## Key Observation

In a rotated sorted array:
- One half is always sorted
- Minimum element lies in the unsorted half

We use Binary Search to eliminate half of the array each time.

---

## Logic

Compare `nums[mid]` with `nums[right]`

### Case 1

```text
nums[mid] > nums[right]
```

Minimum is on the RIGHT side.

So:

```text
left = mid + 1
```

---

### Case 2

```text
nums[mid] <= nums[right]
```

Minimum could be `mid` itself or on LEFT side.

So:

```text
right = mid
```

Continue until:

```text
left == right
```

That index contains minimum.

---

## Pattern

- Binary Search on Rotated Array
- Search Space Reduction
- Sorted Half Identification

---

## Important Learning

Binary Search is not only for finding elements.

It is also used when:
- one half gives useful information
- answer space can be reduced

---

## Complexity

- Time: `O(log n)`
- Space: `O(1)`

---

## Interview Line

We use Binary Search because one side of the rotated array is always sorted. By comparing `mid` with `right`, we can identify where the minimum element must exist and reduce the search space efficiently.

---

## Code

```csharp
public class Solution {

    public int FindMin(int[] nums) {

        int left = 0;
        int right = nums.Length - 1;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[right]) {
                left = mid + 1;
            }
            else {
                right = mid;
            }
        }

        return nums[left];
    }
}
```

