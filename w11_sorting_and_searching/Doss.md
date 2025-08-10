# 10.3 Search in Rotated Array

Given a sorted array of n integers that has been rotated an unknown number of times, write code to find an element in the array. You may assume that the array was originally sorted in increasing order.

EXAMPLE
lnput: find 5 in {15, 16, 19, 20, 25, 1, 3, 4, 5, 7, 10, 14}
Output: 8 (the index of 5 in the array)

## Binary Search

Binary search is most typical way to search a element in sorted array. But the given condition is the array has been rotated certain number of times, so we need to modify classical binary search.

### Binary Search in Rotate Case

Actually, which index is starting point doesn't matter. The point is to find the index range that contains the value we’re looking for. First, let's think about it: if the value at the 'low' index is smaller than the value at the 'mid' index, we can say the range from low to mid is sorted. The reverse is also true: if the value at the 'mid' index is smaller than the value at the 'high' index, then the range from mid to high is sorted. This also means that one side is not sorted, while the other side is sorted.

## Code

```Java
int searchRotated(int[] arr, int val) {
    var l = 0;
    var h = arr.length - 1;

    while (l <= h) {
        int mid = l + (h - l) / 2;

        if (arr[mid] == val) return mid;

        if (arr[l] <= arr[mid]) {
            if (arr[l] <= val && val < arr[mid]) h = mid - 1;
            else l = mid + 1;
        } 
        else {
            if (arr[mid] < val && val <= arr[h]) l = mid + 1;
            else h = mid - 1;
        }
    }

    return -1;
}
```