
# 8.1 Towers of Hanoi

## The Question
In the classic problem of the Towers of Hanoi, you have 3 towers and N disks of
different sizes which can slide onto any tower. The puzzle starts with disks sorted in ascending order
of size from top to bottom (i.e., each disk sits on top of an even larger one). You have the following
constraints:
(1) Only one disk can be moved at a time.
(2) A disk is slid off the top of one tower onto another tower.
(3) A disk cannot be placed on top of a smaller disk.
Write a program to move the disks from the first tower to the last using stacks.

## Questions I Will Ask Before Going into Solution
- Should I guard against illegal moves or is correctness of my algorithm assumed?
- Is there an upper bound on N I should assume?
- Do I receive pre-constructed stack objects, or should I build a Tower and Stack classes myself?

## Recursive Approach
```python
class Tower(object):
    def __init__(self, index):
        self.index = index
        self.disks = []

    def add(self, d):
        if self.disks and self.disks[-1] <= d:
            print "Error placing disk", d
        else:
            self.disks.append(d)

    def moveTopTo(self, dest_tower):
        top = self.disks.pop()
        dest_tower.add(top)

    def moveDisks(self, n, destination, buffer_tower):
        if n > 0:
            # Move n-1 to buffer using destination as temporary
            self.moveDisks(n - 1, buffer_tower, destination)
            # Move top disk to destination
            self.moveTopTo(destination)
            # Move n-1 from buffer to destination using self as temporary
            buffer_tower.moveDisks(n - 1, destination, self)

def main():
    n = 3
    towers = [Tower(i) for i in range(3)]          

    for d in range(n - 1, -1, -1):                 
        towers[0].add(d)

    towers[0].moveDisks(n, towers[2], towers[1])

if __name__ == "__main__":
    main()

```

## Compelxity Analysis
The algorithm’s time complexity is exponential because when we move n disks, we move n-1 disks twice. This means new n is calculated from two (n-1) number of calculations. Thus, the time complexity for this algorithm is O(2^N)
For spatial coplexity, recursion call stack would go no further then depth of n. Also, the storage would be no longer
than n when three stacks are combined. So it will be O(N).
