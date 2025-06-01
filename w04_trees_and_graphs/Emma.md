# Problem 4.3 List of Depths
## Question.
Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth(e.g. if you have a tree with depth D, you'll have D linked lists)

## Approach.
tree traversal. -> track the depth.
We want to create a list of nodes at each depth level, and we’re considering using pre-order traversal for this. Pre-order follows the DLR (Data, Left, Right) order, so we first read the current node’s value (Data), then visit the left subtree, and finally the right subtree.
However, since the problem asks us to group nodes by depth, we need a way to collect nodes into separate lists based on their current depth during traversal.

## Solution.
We can implement a simple modification of the pre-order traversal algorithm, where we pass in level +1 to the next recursive call. 

*using depth-first search*
```
void createLevelLinkedList(TreeNode root, ArrayList<LinkedList<TreeNode>> lists, int level) {
		if(root == null) return; //base case -> leaf node's children is null
		LinkedList<TreeNode> list = null;
		// lists : The lists group the tree nodes by depth.
		if(lists.size() == level) { // the list for that level hasn't been created yet.
			list = new LinkedList<TreeNode>();
			lists.add(list);
		}else {
			list = lists.get(level);
		}
		list.add(root);
		// recursive approach makes DFS possible.
		createLevelLinkedList(root.left, lists, level+1);
		createLevelLinkedList(root.right, lists, level+1);
	}
	ArrayList<LinkedList<TreeNode>> createLevelLinkedList(TreeNode root){
		ArrayList<LinkedList<TreeNode>> lists = new ArrayList<LinkedList<TreeNode>>();
		createLevelLinkedList(root, lists, 0);
		return lists;
	}
```

#### The key knowledge for solving this problem
1. Detph-first search
2. breadth-first search

## Review.
I understand the concept of trees and graphs, but applying them to actual problem is difficult.What I found especially challenging was implementing depth-first search using recursion. Starting from the root, the recursive calls allowed me to go deeper into the left subtree first. When I reached a leaf node with no more children to visit, the function returned and moved back up one level. Since this is done through recursion, the previous function calls were still on the call stack, allowing the algorithm to backtrack and visit the right sibling nodes. In this way, depth-first search was accomplished.

## Compelxity  Analysis
Time Compelxity  : O(N)  
Space Compelxity : Although the recursive approach uses additional space due to the call stack — specifically O(log N) space in the case of a balanced tree — the algorithm still needs to return O(N) data.
Since the returned result contains all the nodes in the tree, it naturally takes up O(N) space.
In terms of Big O notation, we focus only on the dominant term, so the overall space complexity is considered O(N).


## Summary
### Tree
A tree is a data structure composed of nodes.
- Each tree has a root node -> Actually a root node isn't necessary in graph theory, but it's usually how we use trees in programming.
- Root node has zero or more child nodes. Each child node has zero or more child nodes, and so on
- The tree cannot contain cytcles. The nodes may or may not be in a particular order, they could ahve any data type as values, and they may or may not have links back to their parent nodes.

### Graphs
A tree is actually a type of graph, but not all graphs are trees. Simply put, a tree is a connected graphs without cycles.  
**A graph is simply a collection of nodes with edges between (some of) them**  
- graphs can be either directed (like the following graph) or undirected. While directed edges are like a one-way street, undirected edges are like a two-way street.
- The graph might consist of multiple isolated subgraphs. If there is a path between every pair of vertices, it is called a "connected graph"
- the graph can also have cycles. An "acyclic graph" is one without cycles  


**Clarification Question**
1. <u>When given a tree question, many candidates assume the interviewer means a binary search tree. Be sure to ask</u> (binary search tree represents that for each node, its left descendents are less than or equal to the current node, which is less than the right descendents)


# Several Questions
1. "All left descendents <= n < all right descendents. This must be true for each node n" what does that mean?  
-> In this context, n represents not only the root node but also the root of every subtree.


