# 1. Solution Code

```py
class Solution:
    def deleteDuplicatesUnsorted(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Count the frequency of each value
        count = defaultdict(int) ## Import defualtdict to count node counter
        curr = head ## curr pointer to head
        while curr: ## While curr is valid, +1 to the node value
            count[curr.val] += 1
            curr = curr.next ## and move to the next node

        # Remove nodes whose values > 1
        dummy = ListNode(0) ## Set dummy
        dummy.next = head ## and set it right before head pointer to follow it
        prev = dummy 
        curr = head ## and set curr as head again be ready to check

        while curr:
            if count[curr.val] > 1: ## If curr node has > 1 value!?
                # Skip the curr
                prev.next = curr.next ## skip and move to the next
            else:
                # Keep the curr
                prev = curr ## ok, we'll keep it
            curr = curr.next ## move to next node

        return dummy.next ## return next since it's head
```

# 2. Key Points
- Use a hash map to count frequencies, then remove nodes with duplicate values in a second pass.
- Dummy node technique helps safely remove nodes, including the head, without special casing.

# 3. Complexity
Time Complexity: O(n)
Space Complexity: O(n)