# 6.3 Dominos

There is an 8x8 chessboard in which two diagonally opposite corners have been cut off. You are given 31 dominos, and a single domino can cover exactly two squares. Can you use the 31 dominos to cover the entire board? Prove your answer (by providing an example or showing why it's impossible).

## Try Solve

❌⬜⬛⬜⬛⬜⬛⬜  
⬜⬛⬜⬛⬜⬛⬜⬛  
⬛⬜⬛⬜⬛⬜⬛⬜  
⬜⬛⬜⬛⬜⬛⬜⬛  
⬛⬜⬛⬜⬛⬜⬛⬜  
⬜⬛⬜⬛⬜⬛⬜⬛  
⬛⬜⬛⬜⬛⬜⬛⬜  
⬜⬛⬜⬛⬜⬛⬜❌

Let’s assume the chessboard looks like the one above. At first glance, it seems possible to cover the entire board with 31 dominos. But if you try, you'll realize that it’s actually impossible to cover the whole board.

❌🟧🟧🟧🟧🟧🟧🟧  
🟧🟧🟧🟧🟧🟧🟧🟧  
🟧🟧🟧🟧🟧🟧🟧🟧  
🟧🟧🟧🟧🟧🟧🟧🟧  
🟧🟧🟧🟧🟧🟧🟧🟧  
🟧🟧🟧🟧🟧🟧🟧🟧  
🟧🟧🟧🟧🟧🟧🟧😱  
😱🟧🟧🟧🟧🟧🟧❌  

You might be able to cover the orange area with the dominos, but there will always be two single cells left uncovered.

## Hints

Now, let's look the below hint.

- Picture a domino laying down on the board. How many black squares does it cover? How many white squares?

## Solution

Let's try to lay down a domino on the board. It will always take one white cell and one black cell, no matter how you place it. This means we need one white and one black cell for each domino. But the numbers of black and white cells do not match when we cut off two diagonally opposite corners. This means we cannot cover the whole board with dominos.

## Extra

If the number of white and black squares on a chessboard are equal, can we always cover the entire board with 1x2 dominos?

❌⬜⬛⬜⬛⬜⬛❌  
⬜❌❌❌❌❌❌⬛  
⬛⬜⬛⬜⬛⬜⬛⬜  
⬜❌❌❌❌❌❌⬛  
⬛⬜⬛⬜⬛⬜⬛⬜  
⬜❌❌❌❌❌❌⬛  
⬛⬜⬛⬜⬛⬜⬛⬜  
⬜❌❌❌❌❌❌⬛

The diagram above is a counterexample to the question. We not only need equal numbers of white and black squares, but also a well-connected board.