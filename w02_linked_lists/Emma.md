# Problem 2.6 Palindrome

## Summary Linked Lists

A linked list is a data structure that represents a sequence of nodes. In a singly linked list, each node points to the next node in the linked list. A doubly linked list gives each node pointers to both the next node and the previous node.()

Unlike an array, a linked list does not allow direct access by index. To find the Kth element, you have to iterate through K elements, which results in a time complexity of O(K).
Instead, the benefit of a linked list is that you can add and remove items from the beginning of the list in constant time, O(1).

Remember that when you're discussing a linked list in an interview, you must understand whether it is a singly linked list or a doubly linked list.

Recursive Problems
A number of linked list problems rely on recursion. However, recursive algorithms require at least O(n) space, where n is the depth of the recursive call. All recursive algorithms can be implemented iteratively, although the iterative version may be more complex.


## Question.
>Implement a function to check if a linked list is a palindrome.  
*(The list must be the same backwards and forwards.)*
---

## Question before going into a solution.
- We know a size of linked list?


## Approach
The first approach is to reverse the linked list and compare it to the original. If both are identical, it is a palindrome.  
However, if the list is too long, we may want to detect linked lists where the front half of the list is the reverse of the second half. By reversing the front half of the list, we can do that. A stack can accomplish this. 

1. We need to push the first half of the elements onto a stack. We can do this in two different ways, depending on whether or not we know the size of the linked list.

2. If we know the size of the linked list, we can iterate through the first half of the elements in a standard for loop, pushing each elements onto a stack.   
2-1. however, if size of list is odd, we must skip the middle node before comparing the second half.

3. If we don't know the size of the list, we will be using the fast runner/slow runner technique. At each step in the loop, we push the data from the slow runner onto a stack. When the fast runner hits the end of the list, the slow runner will have reached the middle of the linked list. By this point the stack will have all the elements from the front of the linked list, but in reverse order.

### Implement
```
    boolean isPalidrome(LinkedListNode head) {
        LinkedListNode fast = head;
        LinkedListNode slow = head;

        Stack<Integer> stack =  new Stack<Integer>();
        while(fast != null && fast.next !=null){ //check whether fast's reached the end.
            stack.push(slow.data);
            slow = slow.next;
            fast = fast.next.next;
        }
        // if size of list is even, fast will be null. if not, odd number -> skip the middle element.
        if(fast != null) slow = slow.next;

        while(slow !=null){
            int top = stack.pop().intValue();
            if(top != slow.data){
                return false;
            }
            slow = slow.next;
        }
        return true;
    }
```

```
class LinkedListNode:
    def __init__(self, data):
        self.data = data
        self.next = None

def isPalindrome(head: LinkedListNode ) -> bool:
    fast = head
    slow = head
    stack = []

    while fast and fast.next:
        stack.append(slow.data)
        slow = slow.next
        fast = fast.next.next

    if fast:
        slow = slow.next

    while slow:
        top = stack.pop()
        if top != slow.data:
            return False

        slow = slow.next

    return True
```
# complexity
O(n)