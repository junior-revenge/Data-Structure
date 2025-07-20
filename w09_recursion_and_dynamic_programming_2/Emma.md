# Problem 8.5 Recursive Multiply
Write a recursive function to multiply two positive integers without using the operator. You can use addition, subtraction, and bit shifting, but you should minimize the number of those operations.

## small example
2*4 = 2+2+2+2

## Brute Force
make recursive function to call multiple times
```
def recursive(n,t):
    for i in range(t):
        x = recursive(n,t)        
return x+n
```
### mistake
RecursionError: maximum recursion depth exceeded - there isn't any base case


## Solution

```
 int minProduct(int a, int b){
        int bigger = a < b ? b : a;
        int smaller = a < b ? a : b;
        return minProductHelper(smaller, bigger);
    }
    int minProductHelper(int smaller, int bigger){
    
        if(smaller ==0) {
            return 0;
        }else if (smaller ==1){
            return bigger;
        }
        int s = smaller >>1;
        int side1= minProductHelper(s,bigger);
        int side2 = side1;

        if(smaller %2 == 1){
            side2 = minProductHelper(smaller -s, bigger);
        }
        return side1 + side2;
    }
```

### Memoization
```
    int minProduct(int a, int b){
        int bigger = a < b ? b : a;
        int smaller = a < b ? a : b;
        int memo[]= new int[smaller+1]; 
        return minProductHelper(smaller, bigger, memo
        );
    }
    int minProductHelper(int smaller, int bigger, int[] memo){
        if(smaller ==0) {
            return 0;
        }else if (smaller ==1){
            return bigger;
        }else if(memo[smaller] >0){
            return memo[smaller];
        }
        int s = smaller >>1;
        int side1= minProductHelper(s,bigger, memo);
        int side2 = side1;

        if(smaller %2 == 1){
            side2 = minProductHelper(smaller -s, bigger, memo);
        }
        memo[smaller] = side1 + side2;
        return memo[smaller];
    }
```