# 8.7 Permutations without dups

Write a method to compute all permutations of a string of unique characters.

## Approach

Let's think about how to count the number of permutations. Basically, the characters are unique, so we can use the factorial method to count the permutations. But our goal is to get all the permutations, not just count them.

However, look at the following:

N, N - 1, N - 2, ..., 1

First, we generate permutations of size N, then of size N - 1, and so on. We can come up with a recursive approach that starts from N and passes N - 1 as the parameter. But how can we generate different permutations with this approach?

## Backtracking

Backtracking is a type of recursive method that steps back and tries another possibility. Now, let's take a look at the following code:

```java
void tracking(char[] arr, int start, ArrayList<String> ans) {
    var leng = arr.length;

    if(start == leng - 1) {
        ans.add(new String(arr));
        return;
    }

    for(var i=start;i<leng;i++) {
        swap(arr, start, i);
        tracking(arr, start + 1, ans);
        swap(arr, start, i);
    }
}
```

The key concepts to understand in the above code are swapping and iteration. For the first character, there are N possible choices. Then, for each subsequent character, there are N - 1 choices, and so on. We simply swap the characters back to return to the previous state after each recursive call. This approach allows us to generate all permutations.

## Time Complexity

Time complexity is N!, and the depth of recursive is N. 

## The Code

I implemented code following:

```java
ArrayList<String> getPermutations(String str) {
    var ans = new ArrayList<String>();
    
    tracking(str.toCharArray(), 0, ans);

    return ans;
}

void tracking(char[] arr, int start, ArrayList<String> ans) {
    var leng = arr.length;

    if(start == leng - 1) {
        ans.add(new String(arr));
        return;
    }

    for(var i=start;i<leng;i++) {
        swap(arr, start, i);
        tracking(arr, start + 1, ans);
        swap(arr, start, i);
    }
}

void swap(char[] arr, int l, int r) {
    var tmp = arr[l];
    arr[l] = arr[r];
    arr[r] = tmp;
}
```