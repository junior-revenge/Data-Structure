
# 10.1 Sorted Merge

You are given two sorted arrays, A and B, where A has a large enough buffer at the end to hold B. Write a method to merge B into A in sorted order.

```java
void merge(int[] a, int[] b, int lastA, int lastB) {
    int indexA = lastA - 1;
    int indexB = lastB - 1;
    int indexMerged = lastA + lastB - 1;

    while (indexB >= 0) {
        if (indexA >= 0 && a[indexA] > b[indexB]) {
            a[indexMerged--] = a[indexA--];
        } else {
            a[indexMerged--] = b[indexB--];
        }
    }
}
```

---

# Merge Sorted Array

You are given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

Merge `nums1` and `nums2` into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

---

## Problem Summary

Given two sorted arrays `nums1` and `nums2`, merge `nums2` into `nums1` in-place to form one sorted array.

- `nums1` has length `m + n`. The first `m` elements are valid; the rest are placeholders (`0`).
- `nums2` has `n` valid elements.
- Your goal is to modify `nums1` so that it contains the merged and sorted result.

---

## Key Strategy

- The arrays are sorted, so the largest elements are at the end.
- Use two pointers starting from the end of the valid parts.
- Compare from the back and place the larger one at the end of `nums1`.

---

## Algorithm & Data Structures

- **Array**
- **Two-pointer technique**

---

## Important Notes

- Do not simply copy; maintain the sorted order.
- Result must be stored in `nums1`.
- Always fill from the back to avoid overwriting valid values.

---

## Java Example

```java
import java.util.Arrays;

public class MergeSort {
    public static void main(String[] args) {
        int[] nums1 = {4, 5, 6, 0, 0, 0};
        int[] nums2 = {1, 2, 3};
        int m = 3, n = 3;

        int nums1Index = m - 1;
        int nums2Index = n - 1;
        int numsIndex = m + n - 1;

        // Step 1: Compare and fill from the end
        while (nums1Index >= 0 && nums2Index >= 0) {
            if (nums1[nums1Index] > nums2[nums2Index]) {
                nums1[numsIndex--] = nums1[nums1Index--];
            } else {
                nums1[numsIndex--] = nums2[nums2Index--];
            }
        }

        // Step 2: Copy remaining nums2 elements
        while (nums2Index >= 0) {
            nums1[numsIndex--] = nums2[nums2Index--];
        }

        System.out.println("result : " + Arrays.toString(nums1));
    }
}
```

---
