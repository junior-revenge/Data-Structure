# 8.8. Permutations with Duplicates

## Problem Description

Write a method to compute all permutations of a string whose characters are **not necessarily unique**.  
The list of permutations **should not contain duplicates**.

---

## Java Code (Using HashSet to Eliminate Duplicates)

```java
import java.util.HashSet;

public class PermuteNoDup {

    public static void main(String[] args) {
        String input = "axbc";
        HashSet<String> result = new HashSet<>();
        permute("", input, result);

        for (String s : result) {
            System.out.println(s);
        }
    }

    static void permute(String prefix, String remaining, HashSet<String> result) {
        if (remaining.length() == 0) {
            result.add(prefix); 
            return;
        }

        for (int i = 0; i < remaining.length(); i++) {
            char ch = remaining.charAt(i);
            String rest = remaining.substring(0, i) + remaining.substring(i + 1);
            permute(prefix + ch, rest, result);
        }
    }
}
```

---

## Explanation

### How permutation is generated step by step:

- 🔸 `char ch = remaining.charAt(i);`  
  Select the i-th character from the remaining string as the next character to be added to the permutation.

- 🔸 `String rest = remaining.substring(0, i) + remaining.substring(i + 1);`  
  Remove the selected character from the remaining string to avoid using it again in the current branch.

  Example:  
  `remaining = "abc", i = 1 → ch = 'b'`  
  `remaining.substring(0, 1) = "a"`  
  `remaining.substring(2) = "c"`  
  `rest = "ac"`

- 🔸 `permute(prefix + ch, rest);`  
  Add the selected character to the current prefix and continue recursion with the updated remaining string.

Example call tree for `permute("", "axbc")`:

```
permute("", "axbc")
├── a + permute("xbc")
│   ├── x + permute("bc")
│   │   ├── b + permute("c") → "axbc"
│   │   └── c + permute("b") → "axcb"
│   ├── b + permute("xc") → "abxc", "abcx"
│   └── c + permute("xb") → "acxb", "acbx"
```

---

## Java Code (Efficient Approach Using Frequency Map)

```java
import java.util.ArrayList;
import java.util.HashMap;

public class PermutationWithDups {

    public static ArrayList<String> printPerms(String s) {
        ArrayList<String> result = new ArrayList<>();
        HashMap<Character, Integer> map = buildFreqTable(s);
        printPerms(map, "", s.length(), result);
        return result;
    }

    private static HashMap<Character, Integer> buildFreqTable(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        for (char c : s.toCharArray()) {
            if (!map.containsKey(c)) {
                map.put(c, 0);
            }
            map.put(c, map.get(c) + 1);
        }
        return map;
    }

    private static void printPerms(HashMap<Character, Integer> map, String prefix, int remaining, ArrayList<String> result) {
        if (remaining == 0) {
            result.add(prefix);
            return;
        }

        for (Character c : map.keySet()) {
            int count = map.get(c);
            if (count > 0) {
                map.put(c, count - 1); 
                printPerms(map, prefix + c, remaining - 1, result);
                map.put(c, count);     
            }
        }
    }

    public static void main(String[] args) {
        ArrayList<String> permutations = printPerms("aabc");
        for (String perm : permutations) {
            System.out.println(perm);
        }
    }
}
```

---
