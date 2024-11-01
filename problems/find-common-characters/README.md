# [Find common characters](https://leetcode.com/problems/find-common-characters/description/)

### Given a string array words, return an array of all characters that show up in all strings within the words (including duplicates). You may return the answer in any order.


Example 1:

Input: words = ["bella","label","roller"]\
Output: ["e","l","l"]

Example 2:

Input: words = ["cool","lock","cook"]\
Output: ["c","o"]
 

Constraints:

1 <= words.length <= 100\
1 <= words[i].length <= 100\
words[i] consists of lowercase English letters.



## Brute force

How can we find common characters given two string?
1. For each character in string A, check if it exists in string B, if it exists let's add to output
    - If there's N characters, each check if it exists in string B takes O(N) to search the string
    - We can speed up the check by using a hash map 
2. How do we account for duplicate?
    - We can keep track of the count
    - If a word has 2 "l" and another word has 3 "l", we will pick the minimum which is 2
3. Given the frequencies of each string, how can we get the common frequencies out?
   - For each character and its frequency, we take the minimum between the two string
   - If character does not exist in other string, this character is not common to both and excluded
  


### Solution

```python3 []
    def commonChars(self, words: List[str]) -> List[str]:
        common_counts = Counter(words[0])
        for i in range(1, len(words)):
            current_counts = Counter(words[i])
            for letter, count in common_counts.items():
                common_counts[letter] = min(common_counts[letter], current_counts[letter])
        ans = []
        for letter, count in common_counts.items():
            for i in range(count):
                ans.append(letter)
        return ans


```

Time complexity: O(N * W), N is number of words and W is average word length
1. In the first for loop, we had N iterations. Within each iteration, there's a for loop that is up to O(W)
2. In the second for loop, we iterate through the frequency counter and for each count, we append to output. This is in O(W)

Space complexity: O(W), in the worst case this will be the longest word and every word is the same length



### Solution

When we find common elements between 2 string, essentially we are looking for a set intersection. Luckily, python has some shortcuts for this.

We can run set intersection on counter as well.

res = res & Counter(a) gives the set intersection between res and Counter(a)

We then construct the output array using res.elements() which produces all characters in the Counter

The code belows also shows your command and fluency of using the programming language.

```python3 []
    def commonChars(self, A):
        res = Counter(A[0])
        for a in A:
            res &= Counter(a)
        return list(res.elements())
```
## Editor notes

This is a good example of set intersection and frequency counter. 

Since word is all lower-case letters which is O(26), we can use int array to store the frequencies as well



