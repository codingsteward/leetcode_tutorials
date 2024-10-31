# [Valid parentheses](https://leetcode.com/problems/valid-parentheses/description/)

### Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.


Example 1:\
Input: s = "()"\
Output: true

Example 2:\
Input: s = "()[]{}"\
Output: true

Example 3:\
Input: s = "(]"\
Output: false

Example 4:\
Input: s = "([])"\
Output: true

 

Constraints:\
1 <= s.length <= 10^4\
s consists of parentheses only '()[]{}'.



## Brute force

The brute force is surprisingly complicated but mimics how one would solve this without a computer.

For example, we have [ { ( ) } ] as our string.

1. We will start from the left and iterate to the right
2. We discover multiple open parentheses to be matched such as [ { (
3. We encounter our first closing parenthesis ) We then look to the left of this parenthesis and try finding the nearest open parenthesis to match with
4. We found ( and match it. We then encounter }. We skip the previous matched ( ) to arrive at { and it's a match
5. Rinse and repeat unless string is processed
6. It's not valid parenthesis if
    - while a close parenthesis is finding it's match, the nearest open is not same type "( }" 
    - while a close parenthesis is finding it's match, there's no available open parenthesis to match it  "( ) }"
    - if there is still unmatched open parentheses at the end of the string  "( ( )"

```python3 []
    def isValid(self, s: str) -> bool:
        matching = {
            '}' : '{',
            ']' : '[',
            ')' : '(',
        }
        open_parens_matched = [None] * len(s)
        for i, c in enumerate(s):
            if c not in matching: # open brackets mark index as not matched
                open_parens_matched[i] = False
            else:
                j = i - 1
                while j >= 0: # look previous open parens that are not matched
                    if open_parens_matched[j] == False: # found nearest one which is not matched
                        if matching[c] != s[j]: # not the same bracket type i.e invalid parentheses
                            return False
                        open_parens_matched[j] = True # matched successfully, update index
                        break
                    else:
                        j -= 1
                if j < 0: # unable to find an open parenthesis to match current close parenthsis
                    return False
        for i in range(len(s)): # check if there's any open parenthesis that is not matched yet 
            if open_parens_matched[i] == False:
                return False
        return True 
```

Time complexity: O(N^2) because O(N) for loop and within the for loop we do up to O(N) operations.\
This happen for the deeply nested string (((((())))))) where N/2 close parens iterate up to N previous characters to find a matching open parenthesis\
Space complexity: O(N) to store the open_parens_matched state

The above code simulates the brute force solution we will do manually. 


## Stack

This is actually a classic stack application in your introduction to data structures class. Balancing parenthesis is one example of a stack usage.
How could we make use of a stack here?

### Observation 1: For each closing parenthesis, we had to loop up to O(N) to find the closest unmatched open parenthesis

### Observation 2: While looping, we had to skip a lot of characters that were already matched previously to get to the closest unmatched

### Observation 3: If we can remove parentheses every time it matches, we can get the closest unmatched in O(1)
1. String = "[ () () () () () ], if after matching for all the inner () and arriving at ']', skip to the '[' in O(1) since the '()' are all matched already

### Solution

We can keep track of all the open parentheses by adding it to an array.
However, we need to be able to remove elements from the array from the back after we matched parentheses.
This suggests a stack as it can pop the element from the back in O(1)

We make use of a dictionary to store the parentheses mapping for cleaner code

1. Loop through the string
2. Encounter an open parenthesis, add to the stack
3. Encounter a closed parenthesis
   - Check the top of the stack for closest unmatched open parens
       - if it's not the same type, it's invalid
       - if it's the same type it's matching parenthesis, pop it out
4. Once loop is finished, if there's any open parenthesis in stack, means it's invalid as it's not matched


```python3 []
    def isValid(self, s: str) -> bool:
        matching = {
            '}' : '{',
            ']' : '[',
            ')' : '(',
        }
        stack = []
        for c in s:
            if c in matching:
                if stack and stack[-1] == matching[c]:
                    stack.pop()
                else:
                    return False
            else: # open brackets
                stack.append(c)
        return len(stack) == 0
```


Time complexity: O(N), for loop is O(N) and within the for-loop stack.pop() and stack.append(c) are O(1)\
Space complexity: O(N), stack can store up to entire string


### Edge cases
1. More open than closed brackets, and vice versa
2. Mismatched brackets
3. Same number of open and closed but not balanced


# Editor notes

This is a classical problem that applies stack as a data structure. One could memorise the pattern as just use a stack whenever we encounter parentheses\
However, you will discover a lot of parentheses problems turn out to not use a stack. 

I hope the observations help in clarifying why stack is suitable for this. 










