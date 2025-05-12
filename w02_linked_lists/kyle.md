
# 2.5 Sum Lists
Disclaimer: Following document did not use chatGPT or any LLM and solely created by initial draft. Therefore, there could be typos or grammatical errors here and there.

  

## The Question
You have two numbers represented by a linked list, where each node contains a single
digit. The digits are stored in reverse order, such that the 1 's digit is at the head of the list. Write a
function that adds the two numbers and returns the sum as a linked list.

EXAMPLE:  

Input: (7-> 1 -> 6) + (5 -> 9 -> 2).That is,617 + 295.
Output: 2 -> 1 -> 9. That is, 912.

FOLLOW UP: Suppose the digits are stored in forward order. Repeat the above problem.
Input: (6 -> 1 -> 7) + (2 -> 9 -> 5).That is,617 + 295.
Output: 9 -> 1 -> 2. That is, 912.


## Questions I Will Ask Before Going into Solution
- Is there a possibility that there could be leading 0s in the linked list? (e.g. 00312)
- By "numbers" do you refer to integers or should we expect floating point numbers to appear as well?

With given examples, I would assume that they are all integers without leading 0s.
  

## Initial Approach
Since the only return value we need is a boolean that represents whether the given string is a permutation of a palindrome and not the entirety of different palindrome permutation cases, I set up rules that make up a palindrome.
1. One and only one character appears odd number of times and every other character must appear even number of times.
2. Every character must appear even number of times.

```python
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def sum_lists(l1, l2) -> ListNode:
    dummy = ListNode(0)
    current_node = dummy
    carry = 0
    
    while l1 != None or l2 != None or carry != 0:
        l1_val = l1.val if l1 else 0
        l2_val = l2.val if l2 else 0
        
        sum_value = l1_val + l2_val + carry
        carry = sum_value // 10
        
        new_node = ListNode(sum_value % 10)
        
        current_node.next = new_node
        current_node = new_node

        l1 = l1.next if l1 else None
        l2 = l2.next if l2 else None

    return dummy.next
```
  

## Complexity Analysis
Since I am iterating through two linked lists, the time complexity of this solution would be
O(Max(M, N)) provided that M and N are length of two given linked lists. Spatial complexity would be O(1) for the working memory and
O(Max(M, N)) if you think about the number of nodes stored as well.