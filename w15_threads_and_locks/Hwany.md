# 15.4 Deadlock-Free Class

## Solution
### LockFactory 
```java
class LockFactory {
    private static LockFactory instance;
    private LockNode[] locks;
    private Map<Integer, LinkedList<LockNode>> lockOrder = new HashMap<>();

    private LockFactory(int count) {
        locks = new LockNode[count];
        for (int i = 0; i < count; i++) {
            locks[i] = new LockNode(i, count);
        }
    }
    
    public static synchronized LockFactory initialize(int count) {
        if (instance == null) instance = new LockFactory(count);
        return instance;
    }

    // 스레드가 락 순서 선언
    public boolean declare(int ownerId, int[] resources) {
        Map<Integer, Boolean> touched = new HashMap<>();

        // 간선 추가
        for (int i = 1; i < resources.length; i++) {
            LockNode prev = locks[resources[i - 1]];
            LockNode curr = locks[resources[i]];
            prev.joinTo(curr); 
            touched.put(resources[i], false);
        }

        // 사이클 검출
        if (hasCycle(touched, resources)) {
            // 간선 되돌리기 (롤백)
            for (int i = 1; i < resources.length; i++) {
                locks[resources[i - 1]].remove(locks[resources[i]]);
            }
            return false;
        }

        // 선언된 순서를 저장
        LinkedList<LockNode> list = new LinkedList<>();
        for (int r : resources) list.add(locks[r]);
        lockOrder.put(ownerId, list);

        return true;
    }

    private boolean hasCycle(Map<Integer, Boolean> touched, int[] resources) {
        for (int r : resources) {
            if (!touched.get(r) && locks[r].hasCycle(new LockNode.VisitState[locks.length], touched)) {
                return true;
            }
        }
        return false;
    }

    public Lock getLock(int ownerId, int resourceId) {
        LinkedList<LockNode> list = lockOrder.get(ownerId);
        if (list == null || list.isEmpty()) return null;

        LockNode head = list.getFirst();
        if (head.getId() == resourceId) {
            list.removeFirst();
            return head.getLock();
        }
        return null;
    }
}
```
### LockNode
```java
class LockNode {
    enum VisitState { FRESH, VISITING, VISITED }

    private int id;
    private List<LockNode> children = new ArrayList<>();
    private Lock lock = new ReentrantLock();
    private int maxLocks;

    public LockNode(int id, int max) {
        this.id = id;
        this.maxLocks = max;
    }

    public void joinTo(LockNode node) { children.add(node); }
    public void remove(LockNode node) { children.remove(node); }
    public int getId() { return id; }
    public Lock getLock() { return lock; }

    public boolean hasCycle(VisitState[] visited, Map<Integer, Boolean> touched) {
        if (touched.containsKey(id)) touched.put(id, true);

        if (visited[id] == VisitState.VISITING) return true; // cycle
        if (visited[id] == VisitState.FRESH) {
            visited[id] = VisitState.VISITING;
            for (LockNode child : children) {
                if (child.hasCycle(visited, touched)) return true;
            }
            visited[id] = VisitState.VISITED;
        }
        return false;
    }
}
```