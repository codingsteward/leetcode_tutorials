# [Best time to buy and sell stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

### Find max profit you can make from buying and selling a given stock

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

 
Example 1:\
Input: prices = [7,1,5,3,6,4]\
Output: 5\
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.\
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.\

Example 2:\
Input: prices = [7,6,4,3,1]\
Output: 0\
Explanation: In this case, no transactions are done and the max profit = 0.
 

Constraints:\
1 <= prices.length <= 105\
0 <= prices[i] <= 104

## Brute force
We will try all buying and selling combinations and pick the maximum profit out of all these. 

### Solution

For each stock price, I will buy at this price and check the profit if I sell at any other price in the future.
Track the maximum for each sell

Time complexity: O(N^2) nested for loops, each loop has up to N elements
Space complexity: O(1) 

```python3 []
def maxProfit(self, prices: List[int]) -> int:
    max_profit = 0
    for i in range(len(prices)): # try buying at this price, and selling in the future
        for j in range(i+1, len(prices)):
            max_profit = max(max_profit, prices[j] - prices[i])
    return max_profit
```


## One pass

In the brute force solution, we tried all the possible buying and selling. Intuitively, we don't have to try all combinations.

For example, if we buy a stock price at $10, we will mentally gravitate to prices above $10 in the future. Out of all these prices, we only consider the largest.

### Insight 1: We will not get max profit if we buy high and sell low. We need to buy low and sell the highest we can get in the future.

Optimisation:
1. For brute force, we are searching rest of array to find which stock price is the highest. This leads to O(N) time complexity.
2. To speed things up, can we obtain the maximum price in the future for index i in O(1)? We can try pre-computing the answer for each index what is the maximum value after it

```python3 []
def maxProfit(self, prices: List[int]) -> int:
    # compute max price for each index
    max_price = [0] * len(prices)
    max_so_far = prices[-1]
    for i in range(len(prices)-1, -1, -1):
        max_so_far = max(max_so_far, prices[i])
        max_price[i] = max_so_far

    max_profit = 0
    for i in range(len(prices)-1):
        # consider max profit for buying at i and selling for max_price onwards which is pre-computed
        max_profit = max(max_profit, max_price[i+1] - prices[i])
    return max_profit
```

Based on this insight 1, time complexity is O(N) and space complexity is O(N).

Can we optimise this further? Insight 1 assumes we will buy at index i and sell at future indices. We pre-compute the max_price array to achieve O(1) lookup at the cost of space.
Instead of buying at index i and selling at future indices. We can ask ourselves the reverse question. 

### Insight 2: What's the max profit if we sell at index i? We will find the minimum price before index i. 

Optimisation:
1. In order to make maximum profit by selling at index i, we will need to know the lowest stock price before index i
2. We can do the min_price array method as insight 1 but realise that we can actually compute this value on the go while the for-loop is happening
3. Track min_so_far so we can compute the max profit for each index

```python3 []
def maxProfit(self, prices: List[int]) -> int:
    min_so_far = float(inf)
    max_profit = 0
    for price in prices:
        min_so_far = min(min_so_far, price)
        max_profit = max(max_profit, price - min_so_far)
    return max_profit
```

Time complexity: O(N)
Space complexity: O(1)


### Insight 3: If current price is cheaper than previous prices, max profit will not happen on this current price

The optimisations are great so far. However, we can still improve it. Firstly, we don't have to keep updating max_profit.
If price < min_so_far, we know we won't make any max_profit at all so no point re-computing it.

```python3 []
    def maxProfit(self, prices: List[int]) -> int:
        min_so_far = float(inf)
        max_profit = 0
        for price in prices:
            if price < min_so_far: 
                min_so_far = price
            elif price - min_so_far > max_profit: # only update if price > min_so_far and the profit is > max_profit
                max_profit = price - min_so_far
        return max_profit
```
Time complexity is the same. However, it is slightly faster due to less checks. 

## Editor notes
This looks to be a DP tagged problem. However, it is slightly misleading since subproblems are not overlapping. 
Recursion relationship looks to be max_profit(i) = max(max_profit(i-1), prices[i] - min_so_far)


