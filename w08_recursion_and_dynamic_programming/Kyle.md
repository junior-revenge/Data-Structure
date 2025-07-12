
# 8.1 Triple Step

## The Question
A child is running up a staircase with n steps and can hop either 1 step, 2 steps, or 3
steps at a time. Implement a method to count how many possible ways the child can run up the
stairs.


## Questions I Will Ask Before Going into Solution
- What is the maximum and minimum number of steps in the staircase?
(minimum is used for setting initial values while maximum is used for preventing overflows)
- Does changing the order or the number of steps make it a separate case?

## Simple Recursive Approach
```python
def solution(n):

    if n == 0:
        return 1
    elif n < 0:
        return 0
    else:
        return solution(n-3) + solution(n-2) + solution(n-1)
```

## Compelxity Analysis
The algorithm’s time complexity is exponential because it follows a third-order recurrence relation.
Spacial complexity will remain O(N).
![mathematical formula](image.png)


## Memoization Approach(Top-down Dynamic Programming)
```python
from collections import defaultdict

cache = defaultdict(int)         
def memoization_solution(n, cache=cache):
    if n == 0:                    
        return 1
    elif n < 0:                   
        return 0
    elif n in cache and n != 0:   
        return cache[n]
    else:
        val = (memoization_solution(n-1, cache) +
               memoization_solution(n-2, cache) +
               memoization_solution(n-3, cache))
        cache[n] = val
        return val
```

## Compelxity Analysis
With memoisation, each distinct sub-problem k (0 ≤ k ≤ n) is evaluated once. Inside that first evaluation we do only O(1) work (three cached look-ups and two additions). Therefore, the time complexity of this approach is O(N).
Cache dictionary holds up to n integers and the worst call stack depth would be also n. This makes time complexity O(N)


** If you don't use recursion and use bottom-up dynamic programming, it is possible to keep spatial complexity down to O(1) as well.