# Solution 10.2 Group Anagrams
Write a method to sort an array of strings so that all the anagrams are next to each other.

## Brute force
Two words are anagrams if they contain the same characters but in different orders. How can you put characters in order?  

## Solution1. 
```

class AnagramComparator implements Comparator<String>{
    public String sortChar(String s){
        char[] content = s.toCharArray();
        Arrays.sort(content);
        return new String(content);
    }

    @Override
    public int compare(String o1, String o2) {
        return sortChar(o1).compareTo(sortChar(o2));
    }

}

String[] arr = {"dog", "god"};
Arrays.sort(arr, new AnagramComparator());
```

---
## Solution2.
```
 void sort(String[] array){
        HashMapList<String,String> mapList = new HashMapList<>();
        for(String s : array){
            String key = sortChar(s);
            mapList.put(key, s);
        }

        int index = 0;
        for(String key : mapList.keySet()){
            ArrayList<String> list = mapList.get(key);
            for(String t : list){
                array[index] = t;
                index++;
            }
        }
    }

    String sortChar(String s){
        char[] content = s.toCharArray();
        Arrays.sort(content);
        return new String(content);
    }


class HashMapList<T,E> {
    private HashMap<T, ArrayList<E>> map = new HashMap<T, ArrayList<E>>();

    public void put(T key, E item){
        if(!map.containsKey(key)){
            map.put(key, new ArrayList<E>());
        }
        map.get(key).add(item);
    }
    public void put(T key, ArrayList<E> items){
        map.put(key, items);
    }
    public ArrayList<E> get(T key){
        return map.get(key);
    }
    public boolean containsKeyValue(T key, E value){
        ArrayList<E> list = get(key);
        if(list == null) return false;
        return list.contains(value);
    }

    public Set<T> keySet(){
        return map.keySet();
    }
    @Override
    public String toString(){
        return map.toString();
    }
}

```