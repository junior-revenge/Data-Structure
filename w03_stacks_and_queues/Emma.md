# Problem 3.3 Stack of plates

## Summary of Stack and queue
A stack is a data structure that follows the Last In, First Out (LIFO) principle, while a queue is a data structure that follows the First In, First Out (FIFO) principle.

Stack uses the following operations
- pop() : Remove the top item from the stack.
- push(item): Add an item to the top of the stack.
- peek(): Return the top of the stack.
- isEmpty() : Return true if and only if the stack is empty.

Unlikely an array, a stack __does not offer constant-time access to the ith item.__ However, it does allow constant-time adds and removes as it doesn't require shifting elements around.  

Queue uses the following operations
- add(item): Add an item to the end of the list.
- remove(): Remove the first item in the list.
- peek(): Return the top of the queue.
- isEmpty() : Return true if and only if the queue is empty.

## Question. 
__Imagine a (literal) stack of plates. If the stack gets to high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stack exceeds some threshold. Implement a data structure SetOfStacks that mimics this. SetOfStacks should be composed of several stacks and should create a new stack once the previous one exceeds capacity. SetOfStacks.push() and SetOfStacks.pop() should behave identically to a single stack (that is, pop() should return the same values as it would if there were just a single stack).__
> follow up. Implement a function popAt(int index) which performs a pop operation on a specific sub-stack.

## Approach
- before reaching the threshold, we keep pushing to the current stack. As soon as threshold is reached, we create a new stack. However, from the outside, the structure should behave like a single stack.
- When performing pop(), if the current stack becomes empty, we should check if there is a previous stack.
If there is, we shouldn’t return empty. Instead, we move to the previous stack and continue popping data from there.

## Questions
- 각 스택들의 용량은 몇으로 할지?

## Solution
- push() :
push should behave identically to a single stack, which means that we need push() to call push() on the last stack in the array of stacks. if the last stack is at capacity, we need to create a new stack.
- pop() :
it should behave similarly to push() in that it should operate on the last stack. If the last stack is empty(after popping), then we should remove the stack from the list of stacks.

## Implements
```
 public void push(int v){
        Stack last = getLastStack();
        if(last !=null && !last.isFull()){
        // When there is a last stack and it is not full,
        // add the element to the last stack.
            last.push(v);
        }else{
            Stack newStack = new Stack(capacity);
            newStack.push(v);
            stacks.add(newStack);
        }
    }
    public int pop(){
        Stack last = getLastStack();
        if(last == null) throw new EmptyStackException();
        int value = last.pop();
        if(last.size() == 0) stacks.remove(stacks.size()-1);
        return value;
    }

    public Stack getLastStack(){
        if(stacks.size() == 0) {
            return null;
        }
        return stacks.get(stacks.size()-1);
    }
```

## Follow up: Implement popAt(int index)

### Questions before going into a solution.
- I should ask whether all stacks are required to be full, or if it's okay to have partially filled stacks in the middle.

### Implements
1. If empty stacks are allowed, the implementation becomes relatively simpler.
```
public int popAt(int index) {
    if (index < 0 || index >= stacks.size()) {
        throw new IndexOutOfBoundsException("Invalid stack index");
    }

    Stack stack = stacks.get(index);
    if (stack.isEmpty()) {
        throw new EmptyStackException();
    }

    return stack.pop(); 
}
```
> index < 0 || index >= stacks.size() : If an invalid index is passed, an `IndexOutOfBoundsException` will be thrown. So, we check in advance.
<br/>


2. If empty stacks are not allowed
```
class SetOfStacks{
    private int capacity;
    ArrayList<Stack> stacks = new ArrayList<>();
    public SetOfStacks(int capacity){
        this.capacity = capacity;
    }

    public void push(int v){ //same
        Stack last = getLastStack();
        if(last !=null && !last.isFull()){
            last.push(v);
        }else{
            Stack newStack = new Stack(capacity);
            newStack.push(v);
            stacks.add(newStack);
        }
    }
    public int pop(){//same
        Stack last = getLastStack();
        if(last == null) throw new EmptyStackException();
        int value = last.pop();
        if(last.size() == 0) stacks.remove(stacks.size()-1);
        return value;
    }

    public Stack getLastStack(){
        if(stacks.size() == 0) {
            return null; 
        }
        return stacks.get(stacks.size()-1);
    }

    public int popAt(int index){
        return leftShift(index, true);
    }

    public int leftShift(int index, boolean removeTop){
        Stack stack = stacks.get(index);
        int removed_item;
        if(removeTop) removed_item = stack.pop();
        else removed_item = stack.removeBottom();

        if(stack.isEmpty()){
            stacks.remove(index);
        }else if(stacks.size() > index +1){
            int v = leftShift(index+1, false);
            stack.push(v);
        }
        return removed_item;
    }
}
class Stack{
    private int capacity;
    public Node top, bottom;
    public int size = 0;
    public Stack(int capacity){
        this.capacity = capacity;
    }
    public void join(Node above, Node below){
        if(below != null) below.above = above;
        if(above !=null ) above.below = below;
    }
    public boolean push(int v){
        if(size >= capacity) return false;
        size ++;
        Node newNode = new Node(v);
        if(size == 1) bottom = newNode;
        join(newNode,top);
        top = newNode;
        return true;
    }

    public int pop(){
        Node t = top;
        top = top.below;
        size--;
        return t.value;
    }

    public boolean isFull(){
        return size >= capacity;
    }
    
    public boolean isEmpty(){
        return size == 0;
    }
    
    public int removeBottom(){
        Node b = bottom;
        bottom = bottom.above;
        if(bottom != null) bottom.below = null;
        size--;
        return b.value;
    }

    public int size() {
        return this.size;
    }
}

class Node{
    int value;
    Node above;
    Node below;

    public Node(int value){
        this.value = value;
    }
}

```

> In the leftShift function, we pass removeTop as true because when we call popAt(index), we want to pop the top element of the specified stack. If we passed false instead, it would mean we want to remove the bottom element, which is used during the rollover process.

> stacks.size() > index +1 : This means that there is a stack after the given index. If `index + 1` is equal to the size of the stacks, it means the stack at that index is the last one.
