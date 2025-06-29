# 6.4 Ant Collision Probability

## Problem Description
- There are n ants, each sitting on a different vertex of a regular n-gon.
- Each ant randomly chooses either clockwise or counterclockwise.
- All ants start walking at the same time.
- **Goal**: Find the probability that a **collision occurs**.

---

## ✅ Step 1: Total Number of Possible Outcomes
- Each ant has 2 choices: CW or CCW
- With **n ants**, the total number of direction combinations is:

\[
2^n
\]

➡️ This comes from the **Multiplication Principle**: Each independent choice multiplies the outcome space.

---

## ✅ Step 2: When Collision **Does NOT** Happen
- Collision only doesn't happen when **all ants move in the same direction**:
  1. All clockwise (CW)
  2. All counterclockwise (CCW)

\[
\text{Non-collision cases} = 2
\]

---

## ✅ Step 3: Compute the Probability

\[
P(\text{no collision}) = \frac{2}{2^n} = \frac{1}{2^{n-1}}
\]

\[
P(\text{collision}) = 1 - P(\text{no collision}) = 1 - \frac{1}{2^{n-1}}
\]

---

## 🧠 Example (n = 3)
- Total cases: \(2^3 = 8\)
- Non-collision cases: 2

\[
P(\text{no collision}) = \frac{2}{8} = \frac{1}{4}
\]

\[
P(\text{collision}) = 1 - \frac{1}{4} = \frac{3}{4} = 0.75
\]

✅ **Collision probability: 75%**

