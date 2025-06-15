# Problem 5.3 - Flip Bit to Win

You have an integer and you can flip **exactly one bit from 0 to 1**.  
Write code to find the **length of the longest sequence of 1s** you could create.

---

## Example
- **Input:** 1775 (Binary: `11011101111`)
- **Output:** 8  
  → Flipping the single `0` in the middle gives the sequence `11011111111`.

---

## Idea & Explanation

We scan the integer **bit by bit from right to left**, using **bitwise operations** to check whether each bit is `1` or `0`.

### Key Concepts

- **`n & 1`** → Checks if the **rightmost bit** is `1`.
- **`n & 2`** → Checks if the **next rightmost bit** is `1` (i.e., one bit left of current).
- **`n >>= 1`** → Right-shifts `n` to process the next bit.

### Logic Flow

We maintain:
- `count`: current sequence length of `1`s.
- `previous`: previous sequence of `1`s before a single `0`.

When we encounter a `0`:
- If the next bit is also `0`, we cannot connect → `previous = 0`.
- If the next bit is `1`, we can potentially connect two sequences → `previous = count`.

We calculate:
```java
int tempMax = previous + 1 + count;
if (tempMax > maxLength) {
    maxLength = tempMax;
}
---- 
public void FlipBitToWin() {
    int n = 1775; // Binary: 11011101111

    // Edge case: all 1s
    if (~n == 0) {
        System.out.println(32);
        return;
    }

    int previous = 0;
    int maxLength = 1;
    int count = 0;

    while (n != 0) {
        if ((n & 1) == 1) {
            count++;
        } else {
            previous = (n & 2) == 0 ? 0 : count;
            count = 0;
        }

        int tempMax = previous + 1 + count;
        if (tempMax > maxLength) {
            maxLength = tempMax;
        }

        n >>= 1;
    }

    System.out.println("Max length after flipping one 0: " + maxLength);
}
