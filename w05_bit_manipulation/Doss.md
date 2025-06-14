# Problem 5.1 - Insertion

You are given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to insert M into N such that M starts at bit j and ends at bit i. You can assume that the bits j through i have enough space to fit all of M. That is, if M = 10011, you can assume that there are at least 5 bits between j and i. You would not, for example, have j = 3 and i = 2, because M could not fully fit between bit 3 and bit 2.

## Bit Operators

- &  : AND
- |  : OR
- ^  : XOR
- ~  : NOT
- << : Left shifting
- >> : Right shifting

### Useful Techniques

- ~0 : This bit mask will fill with 1s

0000 -> 1111

- (1 << i) - 1 : This bit mask will fill with 1s from 0 to i-1, useful for clearing left side bits

1 << 2 = 0100
0100 - 1 = 0011

- X & mask : This bit mask will clear certain parts of X or all of X

1011 & 1100 = 1000

- X | mask : This bit mask will set certain parts of X or all of X

1000 | 0011 = 1011

## Solution

```java
// function parameters, input m bits into n at position i through j
public static int updateBits(int n, int m, int i, int j)
```

1. Clear bits j through i in N to build mask

```java
    // switch all bits to 1
    int base = ~0;

    // shift 1s to the left of position j, then all bits clear to the right of j
    int right = base << (j + 1);

    // 1. shift 1 bit at position 0 to position i
    // 2. subtract 1 to fill 1 bits from 0 to i-1
    int left = (1 << i) - 1;

    // all bits are 1s except for 0s between i and j
    int mask = right | left;
```

2. Put M bits in the correct position
```java
    // clear bits j through i in n
    int clear = n & mask;

    // move m into correct position
    int shift = m << i;
```

3. Merge M and N
```java
    return clear | shift;
```

