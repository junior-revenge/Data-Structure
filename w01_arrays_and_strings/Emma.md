
# Problem1.8 Zero Metirx
Disclaimer : This is a personal study note created while learning from *Cracking the Coding Interview*. Some parts include direct references from the book and certain explanations were written in my own words for personal understanding.
## Question.
> Write an algorithm such that if an element in an MxN Matrex is 0, its entire row and column are set to 0.
<hr>

## Questions before going into a solution.
- Is there a constraint on space complexity?
- Should I consider only the original zero or the zeros I add while updating?

## Approach
Just iterate through the matrix and every time we see a cell with value zero, set its row and column to 0. 

**posible Problem**  
> All elements can be set to zeros.  
Q. making another matrix to flag? -> space complexity will become O(MN)

## Solution
we can reduce the space to O(1) by using the first row as a replacement for the row array and the first column as a replacement for the column array. This works

> I found this part difficult. So I asked to ChatGPT.  

For example, 
it's an matrix
```
[
  [1, 2, 3],
  [4, 5, 0],
  [7, 8, 9]
]
```  
There is 0 at row 1 column2. To mark this, set **`matrix[1][0] = 0`**, which means that row1 has zero.
Also set **`matrix[0][2]`**,which means column 2 has zero.  
Therefore, To mark which rows and columns contain a zero, we set values in the first column and row of matrix to zero.

## Implement
1. Check if the first row and first column have any zeros and set the variables rowHasZero and columnHasZero.
2. Iterate through the rest of the matrix, setting matrix[i][0] and matrix[0][j] to zero whenever there's a zero in matrix[i][j].
3. Iterate through rest of matrix, nullifying row i if there's a zero in matrix[i][0].
4. Iterate through rest of matrix, nullifying column J if there's a zero in matrix[0][j].
5. Nullify the first row and first column, if necessary

```

    public static int[][] solution(int[][] arrs){
//      initialize
        boolean rowZero = false;
        boolean colZero = false;
//        check if first row has zero
        for(int j = 0; j < arrs[0].length; j++){
            // arrays[0].length : column count. check first row's column
            if(arrs[0][j] ==0){
                rowZero = true;
                break;
            }
        }
//        check if first column has zero.
        for(int i=0; i< arrs.length; i++){
            // arrs.length : row count
            if(arrs[i][0]==0){
                colZero = true;
                break;
            }
        }
//        marking for zeros in the rest of the array
        for(int i = 1; i<arrs.length; i++){
            for(int j = 1; j<arrs[0].length; j++){
                if(arrs[i][j] ==0){
                    arrs[i][0] = 0;
                    arrs[0][j] = 0;
//                    mark the row and column has zero
                }
            }
        }

        for(int i = 0; i<arrs.length; i++){
            if(arrs[i][0] == 0){
                nullifyRow(arrs,i);
            }
        }
        for(int j = 0; j<arrs[0].length; j++){
            if(arrs[0][j] == 0) nullifyCol(arrs,j);
        }
        return arrs;
    }

//    setting to zero
static void nullifyRow(int[][] arrs, int row){
        for(int j = 0; j<arrs[0].length; j++){
            arrs[row][j]=0;
        }
    }
static void nullifyCol(int[][] arrs, int col){
        for(int i=0; i<arrs.length; i++){
            arrs[i][col]=0;
        }
    }
}
```


