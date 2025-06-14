# Problem 5.5 Debugger.
## Questions.
Explain what the following code does : ((n & (n-1)) == 0).<hr/>

## Approach
A & B == 0, then A and B can't share any bits that are both set to 1. <br>
_(Binary bits are indexed starting from 0 on the right, and when two bits are both 1 at the same position, we say they share a 1-bit at that position.)_


### Confusion
Q. Example of this book said 
`if n=abcde1000 then  n-1=abcde0111`.  
And **'abcde must be all 0s.'** Why is that?  

A. For `A & B ==0` to be true, A and B must not share any bits that are set to 1.If any bits in abcde are 1, the result might not to be 0. There fore abcde must be all 0s.  

## Solution
When we think about how we manually perform subtraction, n-1 looks similar to n, n’s initial 0s become 1s in n-1, and n’s least significant (rightmost) 1 becomes a 0.  
n and n-1 must have no 1s in common. Given that they look like this.
> if &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; n=abcde1000<br>
  then  n-1=abcde0111

abcde must be all 0s, which means that n must look like this : 000001000.  
It becomes true only when exactly one bit is set to 1.
Having exactly one bit set means the number is a power of two.
Therefore, the condition is true only when n is a power of two.

## Summary
Bit manipulation means working with the individual bits(0s and 1s) of numbers using bitwise operators. Computers store bumbers as binary and bit manipulation allows us to change or check specific bits directly

_bit and binary_
> A bit is the smallest piece of information in a computer. (eg. 0 or 1)  

> Binary is a number system that uses only bits(0and 1). It's like how we use decimal, but binary uses just 0 and 1.  
(eg. binary number 1011 : made of 4 bits. )


