---
title: Leetcode刷题笔记 2021.08.28
tag: ['leetcode', '动态规划']
categories: 
- leetcode
- dp
---
# Leetcode刷题笔记 2021.08.28

## 动态规划（中等）

#### [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

**尝试通过暴力解法，最后TLE**

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
    let res = -Infinity
    for (let i = 0; i < nums.length; i++) {
        for (let j = i; j < nums.length; j++) {
            if (i == j) {
                if (nums[i] > res) {
                    res = nums[i]

                }
            } else {
                let sum = 0
                for (let k = i; k <= j; k++) {
                    sum += nums[k]
                }
                if(sum>res) {
                    res = sum
                }
            }
        }
    }
    return res
};
```

**通过动态规划求解**

- **状态定义：dp[i]表示以nums[i]结尾的连续子数组的最大和**

- **状态转移方程：**
  - **dp[i - 1] > 0, dp[i] = dp[i - 1] + nums[i]**
  - **dp[i - 1] <=0, dp[i] = nums[i]**
- **初始化：dp[0] = nums[0]**
- **输出：max(dp)**

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
    let len = nums.length
    let dp = []
    dp[0] = nums[0]
    for (let i = 1; i < len; i++) {
        if (dp[i - 1] > 0) {
            dp[i] = dp[i - 1] + nums[i]
        } else {
            dp[i] = nums[i]
        }
    }
    let res = -Infinity
    for (let i = 0; i < len; i++) {
        if (dp[i] > res) res = dp[i]
    }
    return res

};
```

#### [剑指 Offer 47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

**动态规划：**

- **状态定义：dp\[i\]\[j\]表示第i行、第j列拿到最多价值礼物**
- **状态转移方程：**
  - **dp\[i][j] = max(dp\[i-1][j], dp\[i][j-1]) + grid\[i][j]**
- **初始化：dp\[0][0] = grid\[0][0]**

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxValue = function (grid) {
    let m = grid.length
    let n = grid[0].length
    let dp = Array.from(Array(m), () => Array(n))
    let res = 0
    for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
            if (i - 1 < 0 && j - 1 < 0) {
                dp[i][j] = grid[i][j]
                // console.log(dp[i][j], i, j)
                res = Math.max(dp[i][j], res)
                continue
            }
            if (i - 1 < 0) {                
                dp[i][j] = dp[i][j - 1] + grid[i][j]
                // console.log(dp[i][j], i, j)
                res = Math.max(dp[i][j], res)
                continue
            }
            if (j - 1 < 0) {
                dp[i][j] = dp[i - 1][j] + grid[i][j]
                // console.log(dp[i][j], i, j)
                res = Math.max(dp[i][j], res)
                continue
            }           
            dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]
            // console.log(dp[i][j], i, j)
            res = Math.max(dp[i][j], res)
        }
    }
    return res
};
```

