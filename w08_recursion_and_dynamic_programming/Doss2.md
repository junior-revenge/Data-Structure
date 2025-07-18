# 8.7 Permutations without dups

Write a method to compute all permutations of a string of unique characters.

## Commbination Approach

```java
private static ArrayList<String> getPermutations(String str) {
    var ans = new ArrayList<String>();
    
    tracking(str.toCharArray(), 0, ans);

    return ans;
}

static private void tracking(char[] arr, int start, ArrayList<String> ans) {
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

static private void swap(char[] arr, int l, int r) {
    var tmp = arr[l];
    arr[l] = arr[r];
    arr[r] = tmp;
}
```