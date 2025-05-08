
# 1.4 Palindrome Permutation
Disclaimer: Following document did not use chatGPT or any LLM and solely created by initial draft. Therefore, there could be typos or grammatical errors here and there.

  

## The Question
Given a string, write a function to check if it is a permutation of a palindrome.
A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. The palindrome does not need to be limited to just dictionary words.

EXAMPLE:  

Input: Tact Coa  

Output: True (permutations: "taco cat", "atco cta", etc.)  




## Questions I Will Ask Before Going into Solution
- Does blank or a space count as a character that forms a palindrome here?
- Does any special characters or numbers also contribute to the palindrome here?
- Does just one empty space (" ") or null count as a palindrome?

I will assume that a blank is not considered as a character that forms a palindrome through example cases. I will assume special characters or any number will contribute to the palindrome but not a null value.
  

## Initial Approach
Since the only return value we need is a boolean that represents whether the given string is a permutation of a palindrome and not the entirety of different palindrome permutation cases, I set up rules that make up a palindrome.
1. One and only one character appears odd number of times and every other character must appear even number of times.
2. Every character must appear even number of times.

```python
def is_palindrome_permutation(s: str) -> bool:
        if s None:
            return False

        s = s.lower()
        s.replace(" ", "")

        count_map = Counter()
        for ch in s:
            count_map[ch] += 1
            

        odd_number_count = 0
        for i in count_map:
            if count_map[i] % 2 == 0:
                odd_number_count += 1
        
        if odd_number_count > 1:
            return False
        else:
            return True
```
  

## Complexity Analysis
Since I am iterating through the string twice(one for creating hash map and one for actually checking if the counts are odd or even), its time complexity will be O(2N) which is practically O(N).
There is no way to make it better since you need to iterate through the string at least once. 
The spatial complexity would be at works O(k) which is practically O(1). The 'k' here will be the number of distinct characters in the given string 's'.
