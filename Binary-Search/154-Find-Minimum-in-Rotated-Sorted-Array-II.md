# 154. Find Minimum in Rotated Sorted Array II

## Problem

Given a sorted array rotated some number of times, find the minimum element.

Difference from previous problem:
- Array may contain duplicates.

Example:

```text
[2,2,2,0,1]
```

Minimum = `0`

---

## Key Observation

This is still Binary Search.

But duplicates create ambiguity.

Normally:
- one side is clearly sorted

With duplicates:

```text
nums[mid] == nums[right]
```

we cannot determine which side contains the minimum.

So we shrink search space safely:

```text
right--
```

---

## Logic

### Case 1

```text
nums[mid] > nums[right]
```

Minimum is on RIGHT side.

```text
left = mid + 1
```

---

### Case 2

```text
nums[mid] < nums[right]
```

Minimum is on LEFT side including `mid`.

```text
right = mid
```

---

### Case 3

```text
nums[mid] == nums[right]
```

Cannot decide direction because of duplicates.

Safely reduce:

```text
right--
```

---

## Pattern

- Binary Search
- Rotated Sorted Array
- Search Space Reduction
- Handling Duplicates

---

## Important Learning

Duplicates can break normal Binary Search logic.

When comparison gives no useful information:

```text
Shrink search space carefully
```

instead of choosing a side directly.

---

## Complexity

### Average:
- Time: `O(log n)`

### Worst Case:
- Time: `O(n)`

Worst case happens when many duplicates exist:

```text
[1,1,1,1,1]
```

- Space: `O(1)`

---


## Interview Line

We still use Binary Search, but duplicates can make it impossible to determine which half is sorted. When `nums[mid] == nums[right]`, we safely shrink the search space by decrementing `right`.

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
            else if (nums[mid] < nums[right]) {
                right = mid;
            }
            else {
                right--;
            }
        }

        return nums[left];
    }
}
```
