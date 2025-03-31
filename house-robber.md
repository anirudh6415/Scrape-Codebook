# House-Robber

## [House Robber](https://leetcode.com/problems/house-robber/)&#x20;

The House Robber problem is a classic problem where we are given an array of integers representing _the amount of money for each house_, and our task is to determine the _maximum amount of money you can rob tonight without alerting the police_ if **two adjacent houses** were broken into on the same night.

***

```markdown
## Problem Statement
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 2:

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 400
```

***

### Approach

We will solve this problem using three approaches:

#### 1. **Recursive Approach**

The recursive approach is the brute-force solution. For each index, we consider two options:

1. Include the current house in the subsequence by adding a non-adjacent house.
2. Exclude the current element from the subsequence and Check for the previous house.

We then find the maximum length of the subsequence formed by including or excluding the houses.

**Code - Recursive Solution**

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        
        def dfs(idx):
            if idx == 0:
                return nums[idx]

            if idx < 0:
                return 0

            include = nums[idx] + dfs(idx-2)
            exclude = 0 + dfs(idx-1)

            return max(include,exclude)
        
        return dfs(len(nums)-1)
```

***

#### 2. **Memoization Dynamic Programming (DP)**

With the same approach above to avoid the recalculation, we will introduce a dictionary to this solution to avoid re-calculation. Although it is not an optimized solution.

**Code - Memoization DP Solution**

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        memo = {}
        def dfs(idx):
            if idx == 0:
                return nums[idx]

            if idx < 0:
                return 0

            if idx in memo:
                return memo[idx]

            include = nums[idx] + dfs(idx-2)
            exclude = 0 + dfs(idx-1)

            memo[idx] = max(include,exclude)
            
            return memo[idx]
        
        return dfs(len(nums)-1)
```

***

#### 3. **Tabulation Dynamic Programming (DP)**

Bottom-up DP approach, We iteratively find maximum money that can be robbed or not.  We will store the results in a DP table(Dp array).

1. Include the current house: Rob's current house and add money from the last non-adjacent house (i-2).
2. Exclude the current house: Take the maximum money robbed from the previous house (i-1).&#x20;

**Code - Tabulation DP Solution**

<pre class="language-python"><code class="lang-python"><strong>class Solution:
</strong>    def rob(self, nums: List[int]) -> int:

        dp = [0] * len(nums)

        dp[0] = nums[0]

        for i in range(1,len(nums)):
            include = nums[i]
            if i>1:
                include += dp[i-2]
            exclude = 0 + dp[i-1]

            dp[i] = max(include,exclude)
        
        return dp[-1]
</code></pre>

***

4\.  Space optimized:&#x20;

Bottom-up DP approach, We iteratively find maximum money that can be robbed or not.  We are not storing the results in a DP table(Dp array), insted we can use prev, prev2.

1. Include the current house: Rob's current house and add money from the last non-adjacent house (i-2).
2. Exclude the current house: Take the maximum money robbed from the previous house (i-1).&#x20;

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        prev = nums[0]
        prev2 = 0

        for i in range(1,len(nums)):
            include = nums[i]
            if i>1:
                include += prev2
            exclude = prev



            curr = max(include,exclude)
            prev2 = prev
            prev = curr 
        
        return prev
```

### Time Complexity

* **Recursive Approach:** `O(2^n)` Due to the exponential number of calls (without optimization).
* **Memoization DP:**  `O(n)` Since we memoize the results for subproblems, making it more efficient than the naive recursive approach.
* **Tabulation DP:**  `O(n)` as it iterates through the list once and fills up the dp array in **`O(1)`**&#x70;er step.
* **Space optimized**: `O(n)` as it iterates through the list.

### Space Complexity

* **Recursive Approach:** **`O(n)`** (due to recursion stack)
* **Memoization DP:** **`O(n)`** (dictionary storing results for `n` indices)
* **Tabulation DP:** **`O(n)`** (array of size `n`)
* **Space optimized**: `O(1)` (Prev, prev2 elements used instead of array)

***
