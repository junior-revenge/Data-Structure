
A straightforward probability problem.
I'll explain it verbally.


```python
import numpy as np

def prob_game1(p: float) -> float:
    """One shot; need to make it."""
    return p

def prob_game2(p: float) -> float:
    """Three shots; need ≥2 successes."""
    return 3 * p**2 * (1 - p) + p**3          # C(3,2)p²(1-p) + C(3,3)p³

def choose(p: float) -> str:
    g1, g2 = prob_game1(p), prob_game2(p)
    if abs(g1 - g2) < 1e-12:
        return "Tie"
    return "Game 1" if g1 > g2 else "Game 2"

if __name__ == "__main__":
    # showcase a few representative values
    for p in np.linspace(0.1, 0.9, 9):
        print(
            f"p = {p:4.2f} │ "
            f"P(Game 1 win) = {prob_game1(p):5.3f} │ "
            f"P(Game 2 win) = {prob_game2(p):5.3f} │ "
            f"Pick → {choose(p)}"
        )
```