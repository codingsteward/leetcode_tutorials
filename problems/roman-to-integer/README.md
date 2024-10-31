# [Roman to integer](https://leetcode.com/problems/roman-to-integer/description/)

### Given a roman numeral, convert it to an integer.

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

| Symbol | Value |
| - | ---- |
| I | 1  |
| V | 5  |
| X | 10 |
| L | 50 |
| C | 100 |
| D | 500 |
| M | 1000 |


For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. \
X can be placed before L (50) and C (100) to make 40 and 90. \
C can be placed before D (500) and M (1000) to make 400 and 900.\
Given a roman numeral, convert it to an integer.

 

Example 1:\
Input: s = "III"\
Output: 3\
Explanation: III = 3.\

Example 2:
Input: s = "LVIII"\
Output: 58\
Explanation: L = 50, V= 5, III = 3.\

Example 3:
Input: s = "MCMXCIV"\
Output: 1994\
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
 

Constraints:\
1 <= s.length <= 15\
s contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M').\
It is guaranteed that s is a valid roman numeral in the range [1, 3999].



## Brute force

This is an adhoc problem which simply provided the logic for you and you just have to code it out.

The tricky part is identifying when is it subtraction versus adding. We find that there's a pattern. 
If a character A is smaller than a character B immediately after it, we know it's a subtraction B-A


### Solution

We loop through the roman numeral. For each character, we add the corresponding value. 

If we identify a subtraction i.e character is smaller than next character, we will subtract and skip the next char

```python3 []
    def romanToInt(self, s: str) -> int:
        symbolToValue = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
        total, i = 0, 0
        while i < len(s):
            if i + 1 < len(s) and symbolToValue[s[i]] < symbolToValue[s[i+1]]: # I before V
                total += symbolToValue[s[i+1]] - symbolToValue[s[i]]
                i += 2
            else:
                total += symbolToValue[s[i]]
                i += 1
        return total
```
Time complexity: O(s), s = length of string\
Space complexity: O(7), there are 7 roman numerals in hash map only

## Editor notes
This question is more about coding and edge cases than problem solving since the algorithm is already provided i.e steps on converting roman to number. 
