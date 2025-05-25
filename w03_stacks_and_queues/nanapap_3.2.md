# Problem 3.2 - Stack Min

## Interview Insight

This problem tests your ability to enhance a basic data structure (stack) with an efficient additional operation: retrieving the minimum element.

Ask only meaningful questions that directly affect your design. Examples include:

| Question                                                                | Why It Matters                                                  |
| ----------------------------------------------------------------------- | --------------------------------------------------------------- |
| Are duplicate values allowed?                                           | Affects whether we allow same value in min stack multiple times |
| Is `min()` ever called on an empty stack?                               | Affects need for exception handling                             |
| Should `min()` reflect current stack only, or global minimum ever seen? | Clarifies ambiguity in requirements                             |
| Can we use O(N) extra space?                                            | Critical to choosing the right strategy (brute vs optimized)    |

---

## Brute Force Approach

### Idea

* Use a regular stack (list)
* To find the current minimum, scan the entire stack each time `min()` is called

### Code

```python
class BruteMinStack:
    def __init__(self):
        self.stack = []

    def push(self, val):
        self.stack.append(val)

    def pop(self):
        if not self.stack:
            raise Exception("Stack is empty")
        return self.stack.pop()

    def min(self):
        if not self.stack:
            raise Exception("Stack is empty")
        return min(self.stack)  # O(N)

    def peek(self):
        if not self.stack:
            raise Exception("Stack is empty")
        return self.stack[-1]

    def is_empty(self):
        return len(self.stack) == 0
```

### Time Complexity

* push: O(1)
* pop: O(1)
* min: O(N)

---

## Optimized Solution (with Extra Stack)

### Idea

* Use a second stack (`min_stack`) to track the minimum value at each point
* Every time we push a value:

  * If it’s less than or equal to the current minimum, also push it to `min_stack`
* When we pop:

  * If the popped value is equal to the top of `min_stack`, pop from both

### Code

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val):
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self):
        if not self.stack:
            raise Exception("Stack is empty")
        val = self.stack.pop()
        if val == self.min_stack[-1]:
            self.min_stack.pop()
        return val

    def min(self):
        if not self.min_stack:
            raise Exception("Stack is empty")
        return self.min_stack[-1]

    def peek(self):
        if not self.stack:
            raise Exception("Stack is empty")
        return self.stack[-1]

    def is_empty(self):
        return len(self.stack) == 0
```

### Time Complexity

* push: O(1)
* pop: O(1)
* min: O(1)

---

## Comparison

| Feature          | Brute Force           | Optimized (Extra Stack) |
| ---------------- | --------------------- | ----------------------- |
| Time for `min()` | O(N)                  | O(1)                    |
| Extra space used | None                  | O(N) for `min_stack`    |
| Simplicity       | Simple implementation | Slightly more complex   |
| Suitable for     | Quick test cases      | Production or interview |

---

## Summary

* Start with brute-force to show you understand the core idea
* Move to O(1) solution to show performance awareness
* Ask only design-impacting questions (e.g. about duplicates or extra space)
* Avoid obvious questions answered directly in the prompt
