# [Merge sorted array](https://leetcode.com/problems/merge-sorted-array/description/)

### Merge two sorted arrays A and B into array A. Array A has m elements and n zeroes padded at the end. Array B has n elements. 

Constraints
A.length == m + n\
B.length == n\
0 <= m, n <= 200\
1 <= m + n <= 200\
-109 <= A[i], B[j] <= 109

## Brute force/Naive
We can copy the elements in array B to the back of array A and sort array A again. 
This approach is slower than optimal as it does not make use of the arrays that are sorted however the code is short and concise.  

### Solution

For each element in array B, copy to back of array A. Sort array A.

In order to copy over, we need figure out which index of A to start at. 
Since array A has m elements, at the m index it is the right place to start.

Time complexity: O(n) for copy + O((n+m)lg(n+m)) for sorting array A
Space complexity: O(n+m) for sorting algorithm 

```python3 []
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    # copy elements from nums2 to nums1
    for i in range(n):
        nums1[i+m] = nums2[i]
    nums1.sort()
```


## Move largest to end of array 
In brute force solution, we are not making use of the property of sorted arrays. 
Intuitively, when we solve this problem manually, we will shift the larger number to end of array.
We compare both arrays to find the largest number. We then copy it to end of array.
We compare both arrays to find the second largest number. We then copy it to 2nd last slot of array. 
... until all elements are copied over.

### Ideas

1. We will need pointers to the backend of each array as well as a pointer pointing to the available slot of array
2. The loop will end when either array ran out of elements. 

### Solution

Time complexity: O(N+M) for iterating through both nums1 and nums2
Space complexity: O(1) 


Solution One
```python3 []
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    # Set up initial pointers 
    ptr_nums1, ptr_nums2 = m-1, n-1
    copy_ptr = len(nums1) - 1

    # loop until pointers exhausted, check for edge cases 
    while ptr_nums1 >= 0 or ptr_nums2 >= 0:
        # compare which element is larger, copy it over
        if ptr_nums2 < 0 or (ptr_nums1 >= 0 and nums1[ptr_nums1] > nums2[ptr_nums2]):
            nums1[copy_ptr] = nums1[ptr_nums1]
            copy_ptr, ptr_nums1 = copy_ptr - 1, ptr_nums1 - 1
        else: # nums2 has larger element 
            nums1[copy_ptr] = nums2[ptr_nums2]
            copy_ptr, ptr_nums2 = copy_ptr - 1, ptr_nums2 - 1
```
Code improvements
1. Solution One can be clearer and improved upon as ptr_nums2 < 0 can just be skipped entirely since
rest of nums1 is in the right place already so we can skip the copying from nums1 to nums1.
2. Another optimisation is copy_ptr decrements no matter what. We can extract it out. 
```python3 []
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    # Set up initial pointers 
    ptr_nums1, ptr_nums2 = m-1, n-1
    copy_ptr = len(nums1) - 1

    # loop until pointers exhausted, check for edge cases 
    while ptr_nums2 >= 0:
        # compare which element is larger, copy it over
        if ptr_nums1 >= 0 and nums1[ptr_nums1] > nums2[ptr_nums2]:
            nums1[copy_ptr] = nums1[ptr_nums1]
            ptr_nums1 -= 1
        else: # nums2 has larger element 
            nums1[copy_ptr] = nums2[ptr_nums2]
            ptr_nums2 -= 1
        copy_ptr -= 1
```

The last improvement is realising ptr_nums1 and ptr_nums2 are not needed as we can rely on m, n
```python3 []
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    copy_ptr = len(nums1) - 1

    # loop until pointers exhausted, check for edge cases 
    while n > 0:
        # compare which element is larger, copy it over
        if m > 0 and nums1[m-1] > nums2[n-1]:
            nums1[copy_ptr] = nums1[m-1]
            m -= 1
        else: # nums2 has larger element 
            nums1[copy_ptr] = nums2[n-1]
            n -= 1
        copy_ptr -= 1
```

### Edge cases

1. m = 0, n is not 0 => nums1 = [0,0,0], nums2 = [1,2,3]
2. m is not 0, n is 0 => nums1 = [1,2,3], nums2 = []
3. m, n not 0, nums1 contains 0 => nums1 = [0,0,0,0], nums2 = [1,2,3]
4. nums1 is shorter than nums2 and vice versa
5. Same length nums1 and nums2 


### Editorial notes

This question is straight-forward and not so much about problem solving. The focus primarily is on coding and verification. 
You should be able to construct a clean solution i.e not a lot of convoluted if-else and an awareness of the potential edge cases that needs to be accounted for. 


