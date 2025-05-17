# 1. Solution Code

```py
class Solution:
    def deleteDuplicatesUnsorted(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Count the frequency of each value
        count = defaultdict(int)
        curr = head
        while curr:
            count[curr.val] += 1
            curr = curr.next

        # Remove nodes whose values > 1
        dummy = ListNode(0)
        dummy.next = head
        prev = dummy
        curr = head

        while curr:
            if count[curr.val] > 1:
                # Skip the curr
                prev.next = curr.next
            else:
                # Keep the curr
                prev = curr
            curr = curr.next

        return dummy.next
```

# 2. Key Points
- Use a hash map to count frequencies, then remove nodes with duplicate values in a second pass.
- Dummy node technique helps safely remove nodes, including the head, without special casing.

# 3. Complexity
Time Complexity: O(n)
Space Complexity: O(n)