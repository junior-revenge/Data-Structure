# 8.3 Magic Index

Magic Index: A magic index in an array A [ 0... n -1] is defined to be an index such that A[i] = i. Given a sorted array of distinct integers, write a method to find a magic index, if one exists, in array A.

## Brute Force

```java
int magicIndex(int[] array) {
    var siz = array.length;

    for(var i = 0; i<siz;i++) {
        if (array[i] == i) return i;
    }

    return -1;
}
```

This approach is very simple, just iterate through the array until we find the magic number. If we fail to find a magic index, we simply return -1. However, this method is quite inefficient. Let’s look for a better solution.

## Binary Search

```java
int magicIndex(int[] array, int left, int right) {
    if(left > right) return -1;
    
    var index = (left + right) / 2;
    var num = array[index];
    
    if(num == index) return index;
    else if(num > index) return magicIndex(array, left, index - 1);
    else return magicIndex(array, index + 1, right);
}

void main() {
    ...

    var length = array.length;
    var magic = magicIndex(array, 0, length - 1);
}
```

Let’s read the problem description again. The given array is sorted, so we can take advantage of that. When it comes to finding a target value in a sorted array, binary search is the way to go.

If the value at a certain index is greater than the index itself, then we need to search the left side. Conversely, if the value is smaller than the index, we search the right side.

## Follow Up Questions : What if the values are not distinct?

This case is a bit tricky because the standard binary search doesn't work here. Let’s look at the two examples below:

1, 1, 1, 1, 1
4, 4, 4, 4, 4

Let’s slightly modify our function. The array is still sorted, so we can make use of that property.

Suppose the value at a certain index is 1, and the index is 2. That means everything to the left of index 2 must be less than or equal to 1. So there’s still a chance that a magic index exists on the left side, and we need to search it. The same logic applies to the right side when the value is greater than the index.

Let's look at the below code.

```java
int magicIndex(int[] array, int left, int right) {
    if(left > right) return -1;
    
    var index = (left + right) / 2;
    var num = array[index];
        
    if(num == index) return index;

    var leftIndex = Math.min(index - 1, num);
    var lvalue = magicIndex(array, left, leftIndex);

    if(lvalue >= 0) return lvalue;

    var rightIndex = Math.max(index + 1, num);
        
    return magicIndex(array, rightIndex, right);
}
```

In this approach, we search both the left and right sides. The two key ideas behind this method are:

1. The array is sorted, so the elements to the left of a given index are always less than or equal to the value at that index.

2. If the value at a certain index is less than the index, we can skip searching the range between that value and the index. The same logic applies symmetrically to the right side.