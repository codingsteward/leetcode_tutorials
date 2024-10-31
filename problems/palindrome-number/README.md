# [Palindrome number](https://leetcode.com/problems/palindrome-number/description/)
### Given an integer x, return true if x is a palindrome, and false otherwise.

 

Example 1:

Input: x = 121\
Output: true\
Explanation: 121 reads as 121 from left to right and from right to left.

Example 2:

Input: x = -121\
Output: false\
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

Example 3:

Input: x = 10\
Output: false\
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
 

Constraints:\
-2^31 <= x <= 2^31 - 1



## Insights

There are two parts to this question. First part is to know what is a palindrome and how to verify it.

A palindrome is a string that reads the same forward or backwards. "ada" reads the same forward or backwards. Same as "madam" or "racecar"

A typical way to verify a palindrome is just to compare the 1st and last characters, 2nd and 2nd last characters, 3rd and 3rd last characters.. and so on
and check if they are the same. If they are the same they will be equal. Example code is below.

```python3 []
def verifyPalindrome(self, word:str) -> bool:
    start, end = 0, len(word) - 1
    while start < end:
        if word[start] != word[end]:
            return False
    return True
```

There are other ways to doing so. One easy way is to reverse the string and just check if the two strings are the same. 

The second part of this question is the input is a number. We cannot use the typical methods of verifying a palindrome string such as indexing or reversing as integer is not an array. 

### The crux of the question becomes how can you reverse a number?

1. One way is to convert an integer into a string. However, this is not allowed by the question
2. The other way is to realise reversing a number is taking the last digit and shifting it to the front

For example, reverse of 1234 is 4321

Observe that 1234 = 1 * 1000 + 2 * 100 + 3 * 10 + 4
             4321 = 4 * 1000 + 3 * 100 + 2 * 10 + 1


Looks like there's a pattern here which is to make the last digit and multiply it by 1000, second last digit multiply it by 100, third last digit multiply it by 10 etc and add them up

### Problem 1: Getting the last digit
- In order to get the last digit we can use modulo operation. This is a common way to get the last digit of a number
- 1234 % 10 = 4

### Problem 2: How to get the next last digit
- After getting 4 as last digit, how do we get 3?
- We can divide 1234 by 10 to get 123. Then we can run modulo again

### Problem 3: How to figure out multiply by 1000, 100 or 10 etc?
- Observe 4321 = (((4) * 10 + 3) * 10 + 2) * 10 + 1
- This is to say we took the last digit, multiply it by 10 when we add the second last digit
- After that, we took their sum and multiply it by 10.
- Repeat previous step until all the last digits are processed


By breaking down a reverse operation into smaller problems, we can now combine them
```python3 []
def reverseNumber(self, num:int) -> int:
    reversed_number = 0
    while num > 0:
        last_digit = num % 10
        num = num / 10
        reversed_number = reversed_number * 10 + last_digit
    return reversed_number
```

 The above code explains the mechanism of reversing a number. However, it is not complete. There are some edge cases for example negative numbers and numbers ending with 0
 1. For negative numbers like -123, 321 doesn't read as -123
 2. For numbers ending with 0 such as 1230, reverse is 321 which means it will never be a palindrome


Accounting for the edge cases, we can now write the final solution

### Solution
We reverse the number iteratively by taking the last digit and add to the reverse sum * 10
In below solution, we checked for edge cases as well as adding a slight optimisation to only check up till the middle instead of the entire number
1. String is a palindrome if the first half of the string is equal to the reverse of the second half of the string
2. racecar => race = reverse(ecar)


Time complexity: O(N)
Space complexity: O(1) 

```python3 []
    def isPalindrome(self, x: int) -> bool:
        if x < 0: return False
        if x % 10 == 0 and x != 0: return False
        rev = 0
        while rev < x:
            rev = rev * 10 + x % 10
            x = x // 10
        return x == rev or x == rev // 10
```

# Editor notes

Reversing a number might not be a straight forward operation. However, knowing how to get the last digit one at a time is a valuable technique that will be applied in many other questions. 
