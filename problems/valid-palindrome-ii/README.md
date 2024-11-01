# [Valid palindrome II](https://leetcode.com/problems/valid-palindrome-ii/description/)

### Given a string s, return true if the s can be palindrome after deleting at most one character from it.

Example 1:

Input: s = "aba"\
Output: true

Example 2:

Input: s = "abca"\
Output: true\
Explanation: You could delete the character 'c'.

Example 3:

Input: s = "abc"\
Output: false
 
Constraints:

1 <= s.length <= 10^5\
s consists of lowercase English letters.

### What is a palindrome?

a word, phrase, or sequence that reads the same backwards as forwards

Examples: racecar, madam


There are two parts of a question
1. Check if a string is a palindrome
2. Deleting one character from a string

### How to check if a string is a palindrome?

1. Reverse a string. If they are equal, it is a palindrome
2. Iterate from both ends of the string inwards. If every pair of character is the same, it is palindrome


### How to remove a character from a string?
1. This is highly language specific
2. For python3, we will make use of list slicing to remove at index
  - new_string = s[:index] + s[index+1:]
  - We concatenate a string up till index but not including + string from index+1 onwards
  - This will be a new string excluding the character at index

We define these helper functions to check if a string is palindrome as well as deleting character at index
```python3 []
        def isPalindrome(s: str) -> bool:
            left, right = 0, len(s) - 1
            while left < right:
                if s[left] != s[right]:
                    return False
                left, right = left + 1, right - 1
            return True
        
        def removeAtIndex(s: str, i: int) -> str:
            if i == 0: return s[1:]
            if i == len(s) - 1: return s[:i]
            return s[:i] + s[i+1:]

```

## Brute force

Try every possible deletion of 1 character in the string and check if it's still a palindrome

```python3 []

    def validPalindrome(self, s: str) -> bool:
        def isPalindrome(s: str) -> bool:
            left, right = 0, len(s) - 1
            while left < right:
                if s[left] != s[right]:
                    return False
                left, right = left + 1, right - 1
            return True
        
        def removeAtIndex(s: str, i: int) -> str:
            if i == 0: return s[1:]
            if i == len(s) - 1: return s[:i]
            return s[:i] + s[i+1:]

        for i, c in enumerate(s):
            new_string = removeAtIndex(s, i)
            if isPalindrome(new_string):
                return True

        return isPalindrome(s)
```

Time complexity: O(N^2), N possible deletions, each deletion we check palindrome in O(N)


## Brute force improved

We don't have to try every possible deletion. We only have to delete characters that are mismatched causing it to not be a palindrome.

1. If we identify a pair of characters that are not the same, we can consider deleting either the left or right character
2. If after deleting, we can proceed to check if the string is a valid palindrome


```python3 []

    def validPalindrome(self, s: str) -> bool:
        def isPalindrome(s: str) -> bool:
            left, right = 0, len(s) - 1
            while left < right:
                if s[left] != s[right]:
                    return False
                left, right = left + 1, right - 1
            return True
        
        def removeAtIndex(s: str, i: int) -> str:
            if i == 0: return s[1:]
            if i == len(s) - 1: return s[:i]
            return s[:i] + s[i+1:]

        left, right = 0, len(s) - 1
        while left < right:
            if s[left] != s[right]: # mismatch
                delete_left = removeAtIndex(s, left)
                delete_right = removeAtIndex(s, right)
                return isPalindrome(delete_left) or isPalindrome(delete_right)
            left, right = left + 1, right - 1
        return True # no mismatch found
```

Time complexity: O(N), If there's no mismatch, we check entire string of length N. \
If there's a mismatch, we remove character at index in O(N), then check the string again. Since we only do this once, it's still O(N)



## Simulate deletion

We don't actually have to delete the characters! Just simulate it by skipping the character.
1. When we encounter a mismatch, we can check palindrome from left+1 to right to simulate deleting left character or left to right-1 to simulate deleting right character

```python3 []
    def validPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1
        while left < right:
            if s[left] != s[right]:
                return self.checkPalindrome(s, left+1, right) or self.checkPalindrome(s, left, right-1)
            left, right = left + 1, right - 1
        return True

    def checkPalindrome(self, s: str, i: int, j: int) -> bool:
        while i < j:
            if s[i] != s[j]: return False
            i += 1
            j -= 1
        return True
``` 


# Editor notes

The key insight is realising we don't have to delete every character but just mismatch ones. \
Next insight is if there's a mismatch and deleting either left or right character doesn't work, it's not a palindrome \
Finally, we can simulate the deletion

This is a basic problem leading up to a hard problem


