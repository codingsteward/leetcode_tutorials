# [Climbing stairs](https://leetcode.com/problems/climbing-stairs/description/)

### How many distinct ways can you climb up a stairs of n steps, given you take 1 or 2 steps each time

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
 

Constraints:
1 <= n <= 45


## Understand

This is a classical problem for dynamic programming (DP). Instead of thinking and worrying about DP, try understanding if this question can be phrased in recursion terms first. 

Define num_ways(n) as number of distinct ways to climb to the top

At the top floor, how can we reach the top floor? From n-1 and n-2 floors. This is because we only can take 1 or 2 steps each time floor n is reachable only by floor n-1 and n-2.

Then we ask ourselves how many ways can we reach floor n-1 and n-2? For n-1, we can reach from n-2 and n-3. For n-2, we can reach from n-3 and n-4. 


Let's say we can reach floor n-1 in 5 ways and n-2 in 7 ways. Then how many ways can we reach floor n? We add them up to get 5 + 7 = 12 ways to reach floor n.

We can now form a recursive relationship:
num_ways(n) = num_ways(n-1) + num_ways(n-2)

Base case:
2. num_ways(1) = 1 (from 0th floor to 1st floor in 1 step)
3. num_ways(2) = 2 (from 0th floor to 2nd floor in 2 steps and from 1st floor to 2nd floor in 1 step)

## Solve

For most DP questions, the time complexity could be exponential as there are a lot of overlapping subproblems. However, it's easier to reason with and to code out a solution quickly.

After coming up with the basic recursion, we can then optimise (add memoization or using bottom up DP)

### Solution
```python3 []
    def climbStairsRecursion(self, n: int) -> int:
        if n == 1: return 1
        if n == 2: return 2

        # recursive case: you can either take 1 step or 2 step
        return self.climbStairsRecursion(n-1) + self.climbStairsRecursion(n-2)
```

Time complexity: O(2^N), each function call will have 2 function calls. The 2 function calls will create 4 function calls. so we will have 2 * 2 * 2 .. * 2 = 2^N function call 

Space complexity: O(N) for recursion stack depth up to N

### Recursion with memoization
Memoization is basically storing the result of a calculation/function into a table so repeated calls for the same calculation/function will look up the table instead of recomputing.

This will speed up overall time complexity as each repeated calculation will just take O(1) to lookup table

In python3 you can add a decorator @lru_cache to do a similar job as memoization

Example of using lru_cache
```python3 []
    @lru_cache(None)
    def climbStairsRecursion(self, n: int) -> int:
        if n == 1: return 1
        if n == 2: return 2

        # recursive case: you can either take 1 step or 2 step
        return self.climbStairsRecursion(n-1) + self.climbStairsRecursion(n-2)
```

Example of using a self-declared memo table
```python3 []
    def climbStairs(self, n: int) -> int:
        memo = [0] * (n + 1)
        return self.climb_Stairs(0, n, memo)

    def climb_Stairs(self, i: int, n: int, memo: List[int]) -> int:
        if i > n:
            return 0
        if i == n:
            return 1
        if memo[i] > 0:
            return memo[i]
        memo[i] = self.climb_Stairs(i + 1, n, memo) + self.climb_Stairs(
            i + 2, n, memo
        )
        return memo[i]
```

Time complexity: O(N), as there are N distinct states and repeated function calls are cached for quicker lookup

Space complexity: O(N) for memo table/cache and recursion stack

### Bottom-up recursion/dynamic programming

We know the recursive relationship from nth floor to smaller floors n-1 and n-2

num_ways(n) = num_ways(n-1) + num_ways(n-2)

By bottom-up DP, it means we compute the answer from the base case all the way to N instead of N to base case.
1. We start with n = 1 and n = 2
2. Compute num_ways(3) = num_ways(2) + num_ways(1)
3. Compute num_ways(4) = num_ways(3) + num_ways(2)
4. ...
5. Compute num_ways(n) = num_ways(n-1) + num_ways(n-2)

```python3 []
    def climbStairs(self, n: int) -> int:
        if n == 1: return 1
        dp = [0 for i in range(n+1)]
        dp[1] = 1
        dp[2] = 2
        for i in range(3, n+1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n]
```
Time complexity: O(N)

Space complexity: O(N)

### Bottom-up recursion/dynamic programming optimised

Observe that to compute the answer for current floor, we only need 2 variables (n-1 and n-2). Any other floors like 1,2,3 ... are irrelevant by then

This suggests we can do with a lot less space complexity

```python3 []
    def climbStairs(self, n: int) -> int:
        if n == 1: return 1
        first = 1
        second = 2
        for i in range(3, n+1):
            third = first + second
            first = second
            second = third
        return second
```

Time complexity: O(N)

Space complexity: O(1)

Turns out that this is actually fibonacci numbers and you can compute it in a closed form or Binets method. However, for normal interviews you do not have to know them. 

# Editor notes

This is an introduction to recursion and dynamic programming.
1. Understand how to derive recursive relationship
2. Know the brute force recursion
3. Recursion + memo or top-down DP
4. Bottom-up DP
5. Bottom-up DP space optimised



