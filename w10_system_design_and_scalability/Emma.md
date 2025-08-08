# Problem 9.2 Social Network
How we Face-book or LinkedIn? would you design the data structures for a very large social network likeDescribe how you would design an algorithm to show the shortest path between two people.(e.g., Me -> Bob -> Susan -> Jason -> You)
Hint. 270, 285, 304, 321

## Brute Force
DFS or BFS. We should do with BFS for finding shortest path.

### DFS
Depth-first search explores one path as far as it can go, and if it reaches a dead end, it backtracks to try another path. Even if the initial path is incorrect, DFS follows it to the end before exploring alternatives. That’s why it may end up traversing a longer path even when a shorter one exists.



## Solution
- isVisited - hash table
- toVisit - queue
- PathNode - represent the path as we're searching it. storing each Person and the previousNode we visited in this path
- BFSData - classes - to hold the data se need for a breadth-first search


## code
```

class Person{
    private int id;
    private ArrayList<Integer> friends;

    public Person(int id, ArrayList<Integer> friends){
        this.id = id;
        this.friends = friends;
    }
    public int getId(){
        return id;
    }
    public ArrayList<Integer> getFriends(){
        return friends;
    }
}
class PathNode{
    private Person person = null;
    private PathNode previousNode = null;
    public PathNode(Person p, PathNode previous){
        person = p;
        previousNode = previous;
    }
    public Person getPerson(){
        return person;
    }
    public LinkedList<Person> collapse(boolean startsWithRoot){
        LinkedList<Person> path = new LinkedList<>();
        PathNode node = this; // the instance that called collapse
        while(node != null){
            if(startsWithRoot){
                path.addLast(node.person);
            }else{
                path.addFirst(node.person);
            }
            node = node.previousNode;
        }
        return path;
    }
}
class BFSData{
    public Queue<PathNode> toVisit = new LinkedList<PathNode>(); // a LinkedList implementation of a Queue
    public HashMap<Integer, PathNode> visited = new HashMap<>();

    public BFSData(Person root){
        PathNode sourcePath = new PathNode(root, null); // Since the source is the starting point, its previous is null.
        toVisit.add(sourcePath); //"As the initial node to be visited, it is added to the queue
        visited.put(root.getId(), sourcePath); // Since it will be visited immediately, we also add it to visited to prevent revisiting
    }

    public boolean isFinished(){
        return toVisit.isEmpty();
    }
}

     LinkedList<Person> findPathBiBFS(HashMap<Integer, Person> people, int source, int destination){
        BFSData sourceData = new BFSData(people.get(source));
        BFSData destData = new BFSData(people.get(destination));
        while(!sourceData.isFinished() && !destData.isFinished()){
            Person collision = searchLevel(people, sourceData, destData);
            if(collision !=null){
                return mergePaths(sourceData, destData, collision.getId()) ;
            }
            collision = searchLevel(people,destData, sourceData);
            if(collision != null){
                return mergePaths(sourceData,destData, collision.getId());
            }
        }
            return null;
    }
    Person searchLevel(HashMap<Integer, Person> people, BFSData primary, BFSData secondary){
        int count = primary.toVisit.size();
        for(int i = 0; i<count; i++){
            PathNode pathNode = primary.toVisit.poll();
            int personId = pathNode.getPerson().getId();

            if(secondary.visited.containsKey(personId)){
                return pathNode.getPerson();
            }

            Person person = pathNode.getPerson();
            ArrayList<Integer> friends = person.getFriends();
            for(int friendId : friends){
                if(!primary.visited.containsKey(friendId)){
                    Person friend = people.get(friendId);
                    PathNode next = new PathNode(friend, pathNode);
                    primary.visited.put(friendId, next);
                    primary.toVisit.add(next);
                }
            }
        }
        return null; //If we don't return above, it means the friend was not found
    }
    LinkedList<Person> mergePaths(BFSData bfs1, BFSData bfs2, int connection){
        PathNode end1 = bfs1.visited.get(connection);
        PathNode end2 = bfs2.visited.get(connection);
        LinkedList<Person> pathOne = end1.collapse(false);
        LinkedList<Person> pathTwo = end2.collapse(true);
        pathTwo.removeFirst();
        pathOne.addAll(pathTwo);
        return pathOne;
    }

```
