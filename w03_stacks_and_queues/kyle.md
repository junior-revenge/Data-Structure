
# 3.1 Three in One

## The Question
Describe how you could use a single array to implement three stacks.


## Questions I Will Ask Before Going into Solution
- Are there any constraints on how I should lay out three separate stacks within one array?
- Are the three stacks equal in their capacity (as in length or number of elements they can store)?

Since the dynamic length version solution takes way too long for an interview, I assuemd that every stack has fixed size and fixed position in the array. Also, I assuemd that these stacks have same capacity as well.

## Fixed Division Approach
Since the only return value we need is a boolean that represents whether the given string is a permutation of a palindrome and not the entirety of different palindrome permutation cases, I set up rules that make up a palindrome.
1. One and only one character appears odd number of times and every other character must appear even number of times.
2. Every character must appear even number of times.

```python
class FixedMultiStack:
    def __init__(self, stack_capacity, number_of_stacks=3):
        self.number_of_stacks = number_of_stacks
        self.stack_capacity = stack_capacity

        self.values = [None] * (stack_capacity * number_of_stacks)

        self.sizes = [0] * number_of_stacks

    def push(self, stack_num, value):
        if not self._valid_stack(stack_num):
            return
        if self.is_full(stack_num):
            return
        self.sizes[stack_num] += 1
        self.values[self._index_of_top(stack_num)] = value

    def pop(self, stack_num):
        if not self._valid_stack(stack_num):
            return None
        if self.is_empty(stack_num):
            return None
        top_index = self._index_of_top(stack_num)
        value = self.values[top_index]
        self.values[top_index] = None
        self.sizes[stack_num] -= 1
        return value

    def peek(self, stack_num):
        if not self._valid_stack(stack_num):
            return None
        if self.is_empty(stack_num):
            return None
        return self.values[self._index_of_top(stack_num)]

    def is_empty(self, stack_num):
        return self.sizes[stack_num] == 0

    def is_full(self, stack_num):
        return self.sizes[stack_num] == self.stack_capacity

    def _index_of_top(self, stack_num):
        offset = stack_num * self.stack_capacity
        return offset + self.sizes[stack_num] - 1

    def _valid_stack(self, stack_num):
        if 0 <= stack_num < self.number_of_stacks:
            return True
        return False
```
  

## Complexity Analysis
Main overhead happens at self.values array. So, time complexity for creating this stack is O(stack_capacity * number_of_stacks). All the other operations will be kept O(1).
The spatial complexity is also O(stack_capacity * number_of_stacks) for the array itself and other operations take O(1).

