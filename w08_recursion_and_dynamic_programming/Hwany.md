# 8.2 Robot in a Grid

## Problem Description
Imagine a robot sitting on the upper left corner of grid with r rows and c columns.
The robot can only move in two directions, right and down, but certain cells are "off limits" such that
the robot cannot step on them. Design an algorithm to find a path for the robot from the top left to
the bottom right.

---

## Java Code

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Robot {

    static List<String> path = new ArrayList<>();

    public static void main(String[] args) {

        int rows = 3, cols = 3;
        findPath(0, 0, rows, cols);
        Collections.reverse(path);
        System.out.println(path);

    }

    static boolean findPath(int r, int c, int rows, int cols) {

        if(r == rows -1 && c == cols -1) {
            path.add("(" + r + "," + c + ")");
            return true;
        }

        boolean right = false, down = false; 

        if(c + 1 < cols) {
            right = findPath(r, c + 1, rows, cols);
        }

        if(!right && r + 1 < rows) {
            down = findPath(r + 1, c, rows, cols);
        }

        if (right || down) {
            path.add("(" + r + "," + c + ")");
            return true;
        }

        return false; 
    }
    
}
```

---

## Explanation
- `findPath` recursively tries to find a path from the current position `(r, c)` to the destination.
- It first tries moving right, then down if right doesn't work.
- When the destination is reached, it adds that position to the `path`.
- The path is stored in reverse order during recursion, so we use `Collections.reverse(path)` before printing to get the correct order.
- The function returns `true` if a path exists, otherwise `false`.

---
