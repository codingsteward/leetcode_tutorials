# [Longest common prefix](https://leetcode.com/problems/longest-common-prefix/description/)

### Write a function to find the longest common prefix string amongst an array of strings

If there is no common prefix, return an empty string "".

Example 1:

Input: strs = ["flower","flow","flight"]\
Output: "fl"\

Example 2:

Input: strs = ["dog","racecar","car"]\
Output: ""\
Explanation: There is no common prefix among the input strings.
 

Constraints:

1 <= strs.length <= 200\
0 <= strs[i].length <= 200\
strs[i] consists of only lowercase English letters.



## Brute force -- pairwise comparisons

This looks like a complicated problem. Let's break it down.

What is a prefix? A word that is before another word.

What is a common prefix between two strings? A word that is common before both strings.


### Longest prefix for 1 string = entire string
How can we find the longest common prefix for 2 strings?
1. We will check the start of each string if both characters match
2. If they match, we will check the next character and so on
3. Once either string ends or we found a mismatch in character, the prefix found is the longest common prefix

```python3 []
def longestCommonPrefix(word1: str, word2: str) -> str:
    min_len = min(len(word1), len(word2))
    for i in range(min_len):
        if word1[i] != word2[i]:
            break
    return word1[:i] 
```

Time complexity: O(prefix) where prefix is the length of the longest common prefix\
Space complexity: O(prefix) if include the prefix substring created



### How can we extend this to multiple strings?
1. We could do this pairwise comparison
2. LCP(word1, word2 ... wordn) = LCP( LCP( LCP(word1, word2), word3), word4) ... wordn)
3. We will run LCP on word1 and word2, the result of this is a prefix. We then run this prefix with word3 and so on until the last word

```python3 []
def commonPrefix(word1: str, word2: str) -> str:
    min_len = min(len(word1), len(word2))
    for i in range(min_len):
        if word1[i] != word2[i]:
            break
    return word1[:i]


def longestCommonPrefix(self, strs: List[str]) -> str
    common_prefix = strs[0]
    for i in range(1, len(strs)):
        common_prefix = self.commonPrefix(common_prefix, strs[i])
    return common_prefix
```
N is length of strs, M is length of strs[0]\
Time complexity: O(N * M)\
We have O(N) pairwise comparisons of 2 words\
Each pairwise comparison, we run a helper function which is O(prefix)\
In this case, prefix maximum length is length of strs[0]. \
Even if there are longer strings than strs[0], we will only take the shorter of two strings to compare in self.commonPrefix

Space complexity: O(N * M), we can't argue it's O(1) space because each commonPrefix checks return a substring in O(prefix)\
We create new substrings each pairwise comparison. In theory, we could just return the indices of the prefix.

## Brute force -- vertical comparisons

Instead of comparing 2 strings at a time, we can check all N strings for each increment prefix length
1. We check prefix of length 1 across all N strings. If they are the same, we check for length 2
2. We repeat until length M where either string ends or there is a mismatch. In this case, we just return the longest common prefix found
3. After we check for any mismatch, if there isn't any, we will return the first string as it is the longest common prefix for all strings

```python3 []
def longestCommonPrefix(self, strs: List[str]) -> str
    for i in range(len(strs[0])):
        current_char = strs[0][i]
        for word in strs:
            if i == len(word) or word[i] != current_char:
                return strs[0][:i]
    return strs[0] # couldn't find any mismatch
```

# Editor notes
There are a lot of bunch of other algorithms such as binary search, divide and conquer or trie. However, for this question, we do not need them. 

