# House-Robber II

## [House Robber II](https://leetcode.com/problems/house-robber-ii)

For the House Robber II problem, we are given an array of integers representing _the amount of money for each house_, All houses at this place are <kbd>**arranged in a circle**</kbd>**.** and our task is to determine the <kbd>_maximum amount of money_</kbd>_&#x20;you can rob tonight without alerting the police_ if **two adjacent houses** were broken into on the same night.

***

```markdown
## Problem Statement
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
Example 2:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 3:

Input: nums = [1,2,3]
Output: 3
 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 1000
```

***

### Approach

We will solve this problem using three approaches:

It is a similar problem to House robbers, But we have to take the first index and last index array into account as well because of the circular nature of the problem.

#### 1. **Recursive Approach**

The recursive approach is the brute-force solution. For each index, we consider two options:

1. Let's start with dividing the array into 2 parts; One includes zero index element, and the other doesn't.
2. Include the current house in the subsequence by adding a non-adjacent house.
3. Exclude the current element from the subsequence and Check for the previous house.
4. We can return the maximum of both divided arrays.

We then find the maximum length of the subsequence formed by including or excluding the houses.

**Code - Recursive Solution**

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
        def dfs(idx,arr,memo):
            if idx == 0:
                return arr[idx]

            if idx < 0:
                return 0
            
            include = arr[idx]+dfs(idx-2,arr,memo)
            exclude = 0+dfs(idx-1,arr,memo)

            memo[idx]= max(include,exclude)
            return memo[idx]

        arr1 = nums[1:]
        arrl = nums[0:len(nums)-1]
        return max(dfs(len(arr1)-1, arr1, [-1] * len(arr1)),dfs(len(arrl)-1, arrl,[-1] * len(arrl)))
```

<mark style="color:red;">**Although this solution works, the time limit is exceeded. So, to make optimized approaches, we could see other approached solutions below.**</mark>&#x20;

***

#### 2. **Memoization Dynamic Programming (DP)**

With the same approach above to avoid the recalculation, we will introduce a dictionary to this solution to avoid re-calculation. Although it is not an optimized solution.

**Code - Memoization DP Solution**

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]

        def dfs(idx,arr,memo):
            if idx == 0:
                return arr[idx]

            if idx < 0:
                return 0
            if memo[idx] !=-1:
                return memo[idx]
                
            include = arr[idx]+dfs(idx-2,arr,memo)
            exclude = 0+dfs(idx-1,arr,memo)

            memo[idx]= max(include,exclude)
            return memo[idx]

        arr1 = nums[1:]
        arrl = nums[0:len(nums)-1]
        
        return max(dfs(len(arr1)-1, arr1, [-1] * len(arr1)),dfs(len(arrl)-1, arrl,[-1] * len(arrl)))
```

***

#### 3. **Tabulation Dynamic Programming (DP)**

Bottom-up DP approach, We iteratively find maximum money that can be robbed or not.  We will store the results in a DP table(Dp array).

1. Let's start with dividing the array into 2 parts; One includes zero index element, and the other doesn't.
2. Include the current house: Rob's current house and add money from the last non-adjacent house (i-2).
3. Exclude the current house: Take the maximum money robbed from the previous house (i-1).&#x20;
4. We can return the maximum of both divided arrays.

**Code - Tabulation DP Solution**

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]

        def tabulation(arr):
            dp = [0] * len(arr)

            dp[0] = arr[0]

            for i in range(1,len(arr)):
                include = arr[i]
                if i>1:
                    include += dp[i-2]
                exclude = 0 + dp[i-1]

                dp[i] = max(include,exclude)
            
            return dp[-1]

        arr1 = nums[1:]
        arrl = nums[0:len(nums)-1]
        
        return max(tabulation(arr1),tabulation(arrl))
```

***

#### 4. Space Optimized Dp

Bottom-up DP approach, We iteratively find maximum money that can be robbed or not.  We are not storing the results in a DP table(Dp array), insted we can use prev, prev2.

1. Let's start with dividing the array into 2 parts; One includes zero index element, and the other doesn't.
2. Include the current house: Rob's current house and add money from the last non-adjacent house (i-2).
3. Exclude the current house: Take the maximum money robbed from the previous house (i-1).
4. We can return the maximum of both divided arrays.&#x20;

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]

        def space_optim_tab(arr):
            prev = arr[0]
            prev2 = 0

            for i in range(1,len(arr)):
                include = arr[i]
                if i>1:
                    include += prev2
                exclude = prev

                curr_max = max(include,exclude)
                prev2 = prev
                prev = curr_max
            return prev
        
        return max(space_optim_tab(nums[1:]),space_optim_tab(nums[:-1]))
```

***

| Approach        | Time Complexity | Space Complexity |
| --------------- | --------------- | ---------------- |
| Recursion       | O(2ⁿ)           | O(n) (stack)     |
| Memoization     | O(n)            | O(n)             |
| Tabulation      | O(n)            | O(n)             |
| Space Optimized | O(n)            | O(1)             |

***
