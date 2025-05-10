# 1. Solution Code

```py
def one_edit_away(s1, s2):
    # If the length difference is greater than 1, more than one edit is needed
    if abs(len(s1) - len(s2)) > 1:
        return False

    # 1. If same length: Check for at most one character replacement
    if len(s1) == len(s2):
        diff = 0
        for a, b in zip(s1, s2):
            if a != b:
                diff += 1
                if diff > 1:
                    return False
        return True

    # 2. If length differs by 1: Check for one insert or delete
    if len(s1) > len(s2):
        s1, s2 = s2, s1

    i = j = 0
    found_diff = False
    while i < len(s1) and j < len(s2):
        if s1[i] != s2[j]:
            if found_diff:
                return False
            found_diff = True
            j += 1  # Only move the longer string's pointer
        else:
            i += 1
            j += 1
    return True
```

# 2. Key Points
- If they have the same length: allow one replacement
- If their lengths differ by 1: allow one insert/delete
- If the difference is more than 1: return False

# 3. Complexity
Time Complexity: O(n)
Space Complexity: O(1)