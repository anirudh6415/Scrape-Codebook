# Best Time to Buy and Sell Stock with Transaction Fee

## [Best Time to Buy and Sell Stocks with Transaction Fee](best-time-to-buy-and-sell-stock-with-transaction-fee.md)

For the Best Time to Buy and Sell Stock with Transaction Fee problem, we are given an array of integers representing _the price of a given stock on the `ith` day and_ _an integer `fee` representing a transaction fee._ our task is to determine <kbd>the maximum profit</kbd> you can achieve.

***

```markdown
## Problem Statement
You are given an array prices where prices[i] is the price of a given stock on the ith day, and an integer fee representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

Note:

You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
The transaction fee is only charged once for each stock purchase and sale.
 

Example 1:

Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
Example 2:

Input: prices = [1,3,7,5,10,3], fee = 3
Output: 6
 

Constraints:

1 <= prices.length <= 5 * 104
1 <= prices[i] < 5 * 104
0 <= fee < 5 * 104
```

***

### Approach

We will solve this problem using three approaches:

#### 1. **Recursive Approach**

The recursive approach is the brute-force solution. For each index, we consider two options:

1. If we are already holding the stock or not.
   1. If we do not Hold the stock, we may buy a NEW ONE or skip the current stock.
   2. Else, if we hold the stock, we may sell the stock, or we can hold it for longer.
   3. We then find the maximum between (bought, skipped) or (sold, held) stocks.

We find the maximum profit obtained.

**Code - Recursive Solution**

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        if len(prices) == 1:
            return 0
        def dfs(idx, holding):
            if idx == len(prices):
                return 0
            if not holding: 
                buy = dfs(idx+1,1)- prices[idx]
                skip= dfs(idx+1,0)
                return max(buy,skip)
            else:
                sell = dfs(idx+1,0)+prices[idx]- fee
                skip = dfs(idx+1,1)
                return max(sell,skip)
        return dfs(0,0)
```

<mark style="color:red;">**Although this solution works, the time limit is exceeded. So, to make optimized approaches, we could see other approached solutions below.**</mark>&#x20;

***

#### 2. **Memoization Dynamic Programming (DP)**

With the same approach above to avoid the recalculation, we will introduce a 2d array to this solution to avoid re-calculation. Although it is not an optimized solution.

**Code - Memoization DP Solution**

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        if len(prices) == 1:
            return 0
        
        memo = [[-1]* 2 for _ in range(len(prices))]
        def dfs(idx, holding):
            if idx == len(prices):
                return 0

            if memo[idx][holding] !=-1:
                return memo[idx][holding]
            if not holding: 
                buy = dfs(idx+1,1)- prices[idx]
                skip= dfs(idx+1,0)
                memo[idx][holding]= max(buy,skip)
            else:
                sell = dfs(idx+1,0)+prices[idx]- fee
                skip = dfs(idx+1,1)
                memo[idx][holding]= max(sell,skip)

            return memo[idx][holding]
        return dfs(0,0)
```

***

#### 3. **Tabulation Dynamic Programming (DP)**

In recursion, we solved the same problem for making decisions for future days first and then coming backward.

When converting the same approach, <mark style="color:green;">we can reverse the process</mark>, iterating the last day to the first.&#x20;

**Code - Tabulation DP Solution**

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        if len(prices) == 1:
            return 0
        
        dp = [[0]*2 for _ in range(len(prices)+1)]

        for i in range(len(prices)-1,-1,-1):
            
            buy = dp[i+1][1] - prices[i]
            skip= dp[i+1][0]
            dp[i][0]= max(buy,skip)
            
            sell = dp[i+1][0]+prices[i]- fee
            hold = dp[i+1][1]
            dp[i][1]=  max(sell,hold)

        return dp[0][0]
        
```

***

#### 4. Space Optimized Dp

With the same tabulation approach, we could use buy and sell variables to avoid storing it in the array.&#x20;

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:

        buy = 0  
        sell = 0

        for price in prices[::-1]:
  
            buy = max(buy, sell - price)  
            sell = max(sell, buy +price- fee)  
        
        return buy
```

***

| Approach        | Time Complexity | Space Complexity |
| --------------- | --------------- | ---------------- |
| Recursion       | O(2ⁿ)           | O(n) (stack)     |
| Memoization     | O(n)            | O(n)             |
| Tabulation      | O(n)            | O(n)             |
| Space Optimized | O(n)            | O(1)             |

***
