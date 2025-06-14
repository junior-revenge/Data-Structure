
# 5.6 Conversion

## The Question
Write a function to determine the number of bits you would need to flip to convert
integer A to integer B.


## Questions I Will Ask Before Going into Solution
- What is the maximum and minimum boundary of integer we will be handling?
- Are we handling signed integers or unsigned integers?

Originally, I thought those questions mattered. But it turns out it's very simple and
since it does not require any arithmetic operations on integers, we have freedom on 
the type of integer we work on.

## XOR Approach
```python
def bit_swap_required(a, b):
    count = 0
    c = a ^ b
    while c != 0:
        count += c & 1
        c = c >> 1
    return count
```

## Compelxity Analysis
Time complexity will be O(N) where N is the number of bits. Spatial complexity would be simply O(1)

## Counting-Only-Ones Approach
```python
def bit_swap_required(a, b):
    count = 0
    c = a ^ b
    while c != 0:
        count += 1
        c = c & (c - 1)
    return count
```
This apporach counts only 1s because subtracting 1 each time changes 1 at the right end to 0. The number of this incidence adds up to number of 1s.
  

## Complexity Analysis
Technically, this has same complexity of O(N) and O(1) but the time complexity could be half the previous approach on average.