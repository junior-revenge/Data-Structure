
# 4.2 Minimal Tree

## The Question
Given a sorted (increasing order) array with unique integer elements, write an algorithm
to create a binary search tree with minimal height.


## Questions I Will Ask Before Going into Solution
- Does "minimal height" mean as balanced as possible?
- Am I allowed to use recursive version?

I interepreted the question as creating a binary search tree out of an integer array as balanced as possible.
I came up with recursive solution and one that isn't.

## Recursive Approach
```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def sorted_array_to_bst(nums):
    if not nums:
        return None
    
    mid = len(nums) // 2
    root = TreeNode(nums[mid])
    root.left = sorted_array_to_bst(nums[:mid])
    root.right = sorted_array_to_bst(nums[mid+1:])
    return root
```

## Compelxity Analysis
If you sum up the cost of all slicing operations over the whole recursion, it adds up to O(n log n), because at each level of recursion, you process all n elements, and the recursion depth is O(log n). For spatial complexity, Python slicing creates new arrays at each recursion level.

The total space used by all slices throughout the recursion is O(n log n) (since at each level, all slices together add up to n, and there are log n levels).

## Non-recursive approach using a queue
```python
from collections import deque

class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def sorted_array_to_bst_iterative(nums):
    if not nums:
        return None

    mid = len(nums) // 2
    root = TreeNode(nums[mid])

    # queue: (parent_node, left_idx, right_idx, is_left_child)
    queue = deque()
    queue.append((root, 0, mid - 1, True))   # left subtree
    queue.append((root, mid + 1, len(nums) - 1, False))  # right subtree

    while queue:
        parent, left, right, is_left = queue.popleft()
        if left > right:
            continue
        mid = (left + right) // 2
        node = TreeNode(nums[mid])
        if is_left:
            parent.left = node
        else:
            parent.right = node
        queue.append((node, left, mid - 1, True))
        queue.append((node, mid + 1, right, False))

    return root
```
  

## Complexity Analysis
In this approach, we avoid array slicing by working directly with indices and a queue. Each node is created exactly once, and for each node, the algorithm simply calculates the middle index of its assigned range in O(1) time. Since each element in the input array is processed only once and there are no costly subarray copies, the total time complexity is O(n). For spatial complexity, the only extra structures used are the queue (which holds at most O(log n) nodes at any one time, corresponding to the breadth of the tree at a given level) and the tree nodes themselves. The overall space complexity is O(n) by the storage required for the tree nodes.