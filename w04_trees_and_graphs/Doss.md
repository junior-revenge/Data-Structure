# Problem 4.5 - Validate BST(Binary Search Tree)

Implement a function to check if a binary tree is a binary search tree.

## 0. Base code

```cpp
struct Node {
    int data;

    Node* left;
    Node* right;

    Node(int data) : data(data), left(nullptr), right(nullptr) { }
};
```

## 1. Just check left smaller, right bigger

```cpp
bool isBinarySearchTree() {
    if (left) {
        if (left->data >= data || !left->isBinarySearchTree()) return false;
    }

    if (right) {
        if (right->data <= data || !right->isBinarySearchTree()) return false;
    }
}
```

At first glance, everything may appear to be fine. However, upon closer inspection, there's a critical flaw: the range of each node is not properly validated. In other words, simply comparing the values of immediate child nodes is not sufficient to determine whether the tree is a valid Binary Search Tree.

## 2. In order traverse

The main types of tree traversals are pre-order, in-order, and post-order. Among these, in-order traversal is especially useful for checking whether a binary tree satisfies the binary search tree (BST) property. Let's dive into the in-order traversal algorithm.

The traversal order is:

1. traverse(left)
2. visit(node)
3. traverse(right)

The algorithm is simple: first, recursively visit the left subtree, then process the current node, and finally visit the right subtree. Look at the code below.

```cpp
	//in-order traverse
	std::vector<int> inOrderTraversal() const {
		auto v = std::vector<int>();
		this->IotImpl(v);
		return v;
	}

	void IotImpl(std::vector<int>& v) const {
		if (left) left->IotImpl(v);
		
		v.push_back(this->data);

		if (right) right->IotImpl(v);
	}

    bool isBinarySearchTree() const {
        auto vec = inOrderTraversal();
        return std::is_sorted(vec.begin(), vec.end());
    }
```

This is C++ code, but a simplified version. Some parts were omitted, but I believe the main concept is still clearly delivered.

As you can see, in-order traversal visits the nodes in ascending order. So if a binary tree is a valid binary search tree (BST), the traversal result should also be in strictly increasing order.

## 3. Min-Max

Here is another approach to validate a binary search tree. it recursively passes down the valid min and max range to each child node. Let's look at the code below.

```cpp
    bool isBstMinMax() const {
	    return this->IbmImpl(INT_MIN, INT_MAX);
    }

    bool IbmImpl(T min, T max) const {
        if (this->data <= min || this->data >= max) return false;

        if (left) {
            if(left->IbmImpl(min, this->data) == false) 
                return false;
        }

        if (right) {
            if(right->IbmImpl(min, this->data) == false) 
                return false;
        }

        return true;
    }
```

This is another simplified version of the code, but it still delivers the main concept clearly. This approach is quite similar to the first one, which only compares the values of child nodes, but it's more precise. What's interesting is that it starts by passing a minimum and maximum range as parameters in the initial call. Then, those ranges are carefully updated based on the parent node's value as the recursion continues.