# longest-increasing-subsequence

## [Longest Increasing Subsequence (LIS)](https://leetcode.com/problems/longest-increasing-subsequence/)

The Longest Increasing Subsequence (LIS) problem is a classic problem where we are given an array of integers, and our task is to find the length of the longest subsequence that is strictly increasing.

***

<pre class="language-markdown"><code class="lang-markdown"><strong>## Problem Statement
</strong>Given an integer array nums, return the length of the longest strictly increasing subsequence.

Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4
Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1
</code></pre>

***

### Approach

We will solve this problem using three approaches:

#### 1. **Recursive Approach**

The recursive approach is the brute-force solution. For each index, we consider two options:

1. Include the current element in the subsequence if it exceeds the previous element.
2. Exclude the current element from the subsequence.

We then find the maximum length of the subsequence formed by including or excluding the element.

**Code - Recursive Solution**

<pre class="language-python"><code class="lang-python"><strong>class Solution:
</strong>    def lengthOfLIS(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return 1
        

        def dfs(idx , prev):
            
            if idx == len(nums):
                return 0
            include =0
            if prev == -1 or nums[idx] > nums[prev]:
                include = dfs(idx+1,idx)+1

            exclude = dfs(idx+1,prev)
            return max(include,exclude)

        return dfs(0,-1)
</code></pre>

***

#### 2. **Memoization Dynamic Programming (DP)**

In this approach, We have used a dictionary for the same intuition of recursion to avoid re-calculation. Although it is not an optimized solution.

**Code - Memoization DP Solution**

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        def dfs(idx , prev,memo):
            if len(nums) == 1:
                return 1
            if idx == len(nums):
                return 0

            if (idx,prev) in memo:
                return memo[(idx,prev)]

            include =0
            if prev == -1 or nums[idx] > nums[prev]:
                include = dfs(idx+1,idx,memo)+1

            exclude = dfs(idx+1,prev,memo)

            memo[(idx,prev)] = max(include,exclude)
            return memo[(idx,prev)]

        return dfs(0,-1,{})
```

***

#### 3. **Tabulation Dynamic Programming (DP)**



**Code - Tabulation DP Solution**

```python
```

***

### Time Complexity

* **Recursive Approach:** `O(2^n)` Due to the exponential number of calls (without optimization).
* **Memoization DP:**  `O(n^2)` since we memoize the results for subproblems, making it more efficient than the naive recursive approach.
* **Tabulation DP:**&#x20;

***
