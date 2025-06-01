
### 🔹 4.1
**Route Between Nodes: Given a directed graph, design an algorithm to find out whether there is a
route between two nodes.**

We are given a **directed graph** and two nodes: `start` and `end`.
We must design an algorithm to check whether a **path exists** from `start` to `end`.

---

### 🔹 Key Idea

This is a **graph traversal** problem.
We can use either:

* **DFS (Depth-First Search)** – go deep along one path
* **BFS (Breadth-First Search)** – explore level by level

Both need a **visited set** to avoid infinite loops from cycles.

---

### 🔹 Iterative DFS – Java Example

```java
public static boolean hasPath(Node start, Node end) {
    Set<Node> visited = new HashSet<>();
    Stack<Node> stack = new Stack<>();
    stack.push(start);

    while (!stack.isEmpty()) {
        Node current = stack.pop();
        if (current == end) return true;
        if (visited.contains(current)) continue;

        visited.add(current);
        for (Node neighbor : current.neighbors) {
            stack.push(neighbor);
        }
    }
    return false;
}
```

---

### 🔹 When to Use DFS or BFS?

| Case                          | Use                    |
| ----------------------------- | ---------------------- |
| Just check if a path exists   | DFS or BFS ✅           |
| Avoid recursion (stack depth) | BFS or Iterative DFS ✅ |
| Need shortest path            | BFS ✅                  |



