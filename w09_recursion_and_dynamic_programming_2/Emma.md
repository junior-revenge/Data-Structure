# Problem 8.5 Recursive Multiply
Write a recursive function to multiply two positive integers without using the operator. You can use addition, subtraction, and bit shifting, but you should minimize the number of those operations.

## small example
2*4 = 2+2+2+2

## Brute Force
make recursive function to call multiple times
재귀함수를 곱하는것마냥 더하기로 리컬시브 만들면?
그럼 우선 더할 인자를 매개변수로 받아(n으로) 그걸 곱해지는 수만큼 호출하면? 즉 2*4는 2를 4번 더한것과 같으니까 두 수를 매개변수로 받고 뒤에 times로 받은 매개변수만큼 호출하는 함수를 만들면?
근데 그걸 t번어떻게 호출하지? 어떻게 그게 t번째라는걸 인식하게 하지?
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