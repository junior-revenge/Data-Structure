# Problem 8.4 Power Set
Write a method to return all subsets of a set

# Brute force
- The number of subsets of a set is 2ⁿ, since each element in the set has two possibilities — either it is included in a subset or it is not.  
- The subsets of a set with n elements can be built from the subsets of a set with n – 1 elements.


# Solution
```
  private static ArrayList<ArrayList<Integer>> solution(ArrayList<Integer> set, int index){
        ArrayList<ArrayList<Integer>> allSubset;
        if(set.size() == index){
            allSubset = new ArrayList<ArrayList<Integer>>();
            allSubset.add(new ArrayList<>());
        }else{
            allSubset = solution(set,index+1);
            int item = set.get(index);
            ArrayList<ArrayList<Integer>> moresubset = new ArrayList<ArrayList<Integer>>();
            for(ArrayList<Integer> subset : allSubset){
                ArrayList<Integer> newsubset = new ArrayList<>();
                newsubset.addAll(subset);
                newsubset.add(item);
                moresubset.add(newsubset);
            }
            allSubset.addAll(moresubset);
        }
        return  allSubset;
    }
```


# Summary of Recursion and Dynamic Programming
Three of the most common approaches to develop an algorithm
1. **Bottom-up Approach**  
We start with knowing how to solve the problem for a simple case, like a list with only one element. Then we figure out how to solve the problem for two elements, then for three elements, and so on. The key here is to think about how you can build the solution for one case off of the previous case

2. **Top-down Approach**  
In these problems, we think about how we can divide the problem for case N into subproblems. Be careful of overlap between the cases.

3. **Half-and-Half Approach**  
In addition to top-down and bottom-up approaches, it's often effective to dive the data set in half. For example, binary search works with a "half-and-half" approach. When we look for an element in a sorted array, we first figure out which half of the array contains the value. 

## Recursive vs iterative Solution
Recursive algorithms can be very space inefficient. Ecah recursive call adds a new layer to the stack. which means that if your algorithm recurses to a depth of n, it uses at least O(n) memory. For this reason, it's often better to implement a recursive algorithm iteratively. All recursive algorithms can be implemented iteratively, although sometimes the code to do so is much more complex.

## Dynamic Programming & Memoization
Dynamic programming is mostly just a matter of taking a recursive algorithm and finding the overlapping subproblmes(that is, the reapeated calls). You can cache those results for future recursive calls. 