# Problem 4.5 - Validate BST(Binary Search Tree)

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
    if(left != nullptr) {
        if(left->data > data) return false;
        left->isBinarySearchTree();
    }

    if(right != nullptr) {
        if(left->data < data) return false;
        left->isBinarySearchTree();
    }
}
```

## 2. In order traverse

```cpp
// in-order traverse approach
std::tuple<int, bool> isBinarySearchTree() {
    if(!left) left->isBinarySearchTree();

    if(!right) right->isBinarySearchTree();
}
```

## 3. Min-Max

```cpp

```

## 4. ETC - Binary Search Tree problem