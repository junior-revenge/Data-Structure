
# 10.4 Sorted Search, No Size

## The Question
Sorted Search, No Size: You are given an array-like data structure Listy which lacks a size
method. It does, however, have an elementAt(i) method that returns the element at index i in
0(1) time. If i is beyond the bounds of the data structure, it returns -1. (For this reason, the data
structure only supports positive integers.) Given a Listy which contains sorted, positive integers,
find the index at which an element x occurs. If x occurs multiple times, you may return any index.
any mechanism you choose.

## Questions I Will Ask Before Going into Solution
- Could Listy be extremely large (e.g., millions or billions of elements)? This impacts whether an O(log n) approach is acceptable.
- If there are multiple occurrences of x, do you need the first/last index, or is any index fine?
- Is elementAt guaranteed to be called only with non-negative indices, or do I need to guard against negatives?

## Approach
1. Find a search bound exponentially.
Start with idx = 1 and double (1, 2, 4, 8, ...) until elementAt(idx) == -1 (past end) or elementAt(idx) >= x.
This gives a range (idx/2, idx] that must contain x if it exists.
2. Binary search within the found window.
```python

def search_listy(listy: "Listy", x: int) -> int:
    first = listy.elementAt(0)
    if first == -1 or x < first:
        return -1
    if first == x:
        return 0

    # 1) Find an upper bound
    idx = 1
    while True:
        val = listy.elementAt(idx)
        if val == -1 or val >= x:
            break
        idx <<= 1

    # Search window is (idx//2, idx]
    lo = (idx // 2) + 0
    hi = idx

    # 2) Binary search
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        v = listy.elementAt(mid)
        if v == -1 or v > x:
            hi = mid - 1
        elif v < x:
            lo = mid + 1
        else:
            return mid

    return -1

```

## Compelxity Analysis
Time:
Exponential bound finding takes O(log p) where p is the index of x (or where x would be).
Binary search within that window is also O(log p).
Overall O(log n) where n is the (unknown) number of elements.

Space:
O(1) auxiliary space.
