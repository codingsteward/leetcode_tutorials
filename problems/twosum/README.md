# [Two Sum](https://leetcode.com/problems/two-sum/description/)

### Find two indices i and j in the array of integers A s.t A[i] + A[j] = target

N = length of array A 
Constraints:
2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.


## Brute force
A complete search algorithm will try out all possible indices i, j and check if they sum up to target. 

### Solution

For each element X, loop the rest of the array and find it they sum up to target. This will be O(N) elements, each running a for loop of O(N) search 

Time complexity: O(N^2)
Space complexity: O(1) 

```python3 []
def twoSum(self, nums: List[int], target: int) -> List[int]:
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```


## Single pass with hash map
In brute force solution, for each element X, we ran a for-loop to find the complement element. If we can optimise this, we can achieve a faster time complexity. 

### Ideas

What is the complement element? It’s target - X
How can we find the complement element?
1. Complete search (as per brute force) O(N)
2. Binary search (it’s not sorted) O(lg N)
3. Use hashmap to store all the elements for O(1) lookup

### Solution

Hashmap stores elements we encounter up till current iteration. For each element X, check hashmap if complement exists. Add X to hashmap

Time complexity: O(N) 
Space complexity: O(N) for hashmap


```python3 []
def twoSum(self, nums: List[int], target: int) -> List[int]:
    seen = {}
    for i in range(len(nums)): # for each element, check in complement exists 
        complement = target - nums[i]
        if complement in seen:
            return [seen[complement], i]
        seen[nums[i]] = i
    return [] # No pairs can be found 
```
