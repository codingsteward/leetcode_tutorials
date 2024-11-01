# [Sort array by increasing frequency](https://leetcode.com/problems/sort-array-by-increasing-frequency/description/)

### Write a custom function to sort array by increasing frequency

Given an array of integers nums, sort the array in increasing order based on the frequency of the values. If multiple values have the same frequency, sort them in decreasing order.

Return the sorted array.

Example 1:

Input: nums = [1,1,2,2,2,3]\
Output: [3,1,1,2,2,2]\
Explanation: '3' has a frequency of 1, '1' has a frequency of 2, and '2' has a frequency of 3.

Example 2:

Input: nums = [2,3,1,3,2]\
Output: [1,3,3,2,2]\
Explanation: '2' and '3' both have a frequency of 2, so they are sorted in decreasing order.

Example 3:

Input: nums = [-1,1,-6,4,5,-6,1,4,1]
Output: [5,-1,4,4,-6,-6,1,1,1]
 

Constraints:

1 <= nums.length <= 100\
-100 <= nums[i] <= 100



## Solution

This problem makes use of 2 concept which are very important
1. Hash map to count frequency of elements
2. Custom sort comparator

Most programming languages would have a sort function. The sort function also takes in a custom sort comparator which sorts array based on your criteria rather than the default way.

When providing a custom comparator, it should generally return an integer/float value that follows the following pattern (as with most other programming languages and frameworks):
1. return a negative value (< 0) when the left item should be sorted before the right item
2. return a positive value (> 0) when the left item should be sorted after the right item
3. return 0 when both the left and the right item have the same weight and should be ordered "equally" without precedence

Do lookup for your chosen programming language syntax and how to use a custom sort comparator

In this case, we are ordering the array elements based on frequency of values. So a smaller frequency will come first and a larger frequency will come later.

If they have the same frequency, we will sort based on decreasing order

Example of comparator
```python3 []
def compare(num1, num2):
    if freq[num1] == freq[num2]:
        return num1 > num2
    return freq[num1] < freq[num2]
```

For python3 we can make use of the key to decide what to sort on
1. frequency of x
2. Decreasing order of x

```python3 []
    def frequencySort(self, nums: List[int]) -> List[int]:
        counter = Counter(nums)
        return sorted(nums, key=lambda x: (counter[x], -x))
```

We compute the frequency of each element and stored in a dictionary. This is done by the Counter object.

Next, we run sorted on the array with a custom key. The custom key says sort based on this key.
1. Firstly, try to sort based on counter[x] which is the frequency of element x
2. If there is a tie, sort based on -x which is the decreasing order


Time complexity: O(N lg N), sorting algorithm time complexity\
Space complexity: O(N), depending on sort implementation


# Editor notes

This is ad-hoc problem that aims to understand if you know how to use a custom sort comparator. It is more about language specs than problem solving.

It also illustates the conciseness of python and why it's a good language to use for interviews.
