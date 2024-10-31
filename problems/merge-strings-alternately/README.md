# [Merge strings alternately](https://leetcode.com/problems/merge-strings-alternately/description/)

### Merge two strings alternately

You are given two strings word1 and word2. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the merged string.

Example 1:\
Input: word1 = "abc", word2 = "pqr"\
Output: "apbqcr"\
Explanation: The merged string will be merged as so:\
word1:  a   b   c\
word2:    p   q   r\
merged: a p b q c r

Example 2:\
Input: word1 = "ab", word2 = "pqrs"\
Output: "apbqrs"\
Explanation: Notice that as word2 is longer, "rs" is appended to the end.\
word1:  a   b \
word2:    p   q   r   s\
merged: a p b q   r   s

Example 3:\
Input: word1 = "abcd", word2 = "pq"\
Output: "apbqcd"\
Explanation: Notice that as word1 is longer, "cd" is appended to the end.\
word1:  a   b   c   d\
word2:    p   q \
merged: a p b q c   d

Constraints:\
1 <= word1.length, word2.length <= 100\
word1 and word2 consist of lowercase English letters.


## Brute force with 2 pointers
This is a straight-forward question that the brute force is the optimal solution. You just have to code out the steps they mention in the question.

### Solution

Have a pointer to each word, add character from word1 first to string builder/array then add character from word2. Check that indices do not exceed boundaries

Time complexity: O(M + N) // to build a string of length(M+N)
Space complexity: O(1) // ignoring res array as it's used for output

```python3 []
def mergeAlternately(self, word1: str, word2: str) -> str:
    ptr_1 = ptr_2 = 0
    res = []
    while ptr_1 < len(word1) or ptr_2 < len(word2):
        if ptr_1 < len(word1):
            res.append(word1[ptr_1])
            ptr_1 += 1
        if ptr_2 < len(word2):
            res.append(word2[ptr_2])
            ptr_2 += 1
    return "".join(res)
```


## Brute force with 1 pointers
Realise that ptr 1 and ptr 2 are essentially the same value until whichever longer word continues beyond
We can just use one pointer and only append the character if not out of bounds of current word

### Solution

```python3 []
def mergeAlternately(self, word1: str, word2: str) -> str:
    max_length = max(len(word1), len(word2))
    res = []
    for i in range(max_length):
        if i < len(word1):
            res.append(word1[i])
        if i < len(word2):
            res.append(word2[i])
    return "".join(res)
```

## Edge cases
1. word1 and word2 are empty
2. word1 is shorter than word2, vice versa
3. word1 is empty or word2 is empty

# Editor notes

Python does not have mutable strings or string builder class like Java. The canonical way of building a string is to have an array and run "".join(array) to build a string.

This is a straight forward question and the focus is on correctness of code and edge cases

