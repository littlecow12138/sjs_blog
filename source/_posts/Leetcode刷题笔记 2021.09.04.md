---

title: Leetcode刷题笔记 2021.09.04

tag: ['leetcode', '双指针', '搜索与回溯算法']

categories: 

- leetcode
- 双指针
- 搜索与回溯算法

---
# Leetcode刷题笔记 2021.09.04

## 双指针

#### [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

**PS: JS解构赋值交换两个变量的值**

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var exchange = function(nums) {
    let left = 0;
  let right = nums.length - 1;
  while (left < right) {
    if (nums[left] & 1) {
      left++;
      continue;
    }
    [nums[right], nums[left]] = [nums[left], nums[right]];
    right--;
  }
  return nums;
};
```

#### [剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function (nums, target) {
    let left = 0
    let right = nums.length - 1
    while (left < right) {
        if (nums[left] + nums[right] > target) {
            right--
        } else if (nums[left] + nums[right] < target) {
            left++
        } else {
            return [nums[left], nums[right]]
        }
    }
    return null
};
```

#### [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

```js
/**
 * @param {string} s
 * @return {string}
 */
var reverseWords = function (s) {
    let re = s.split(' ').reverse()
    // console.log(re)
    let res = []
    for (let i = 0; i < re.length; i++) {
        if (re[i] == '') continue
        res.push(re[i])
    }
    return res.join(' ')
};
```

## 搜索与回溯算法（中等）

#### [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

**深度优先搜索（DFS）+ 剪枝**

```js
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function (board, word) {
    let rows = board.length
    let cols = board[0].length || 0

    let dfs = function (i, j, board, word, index) {
        if (i < 0 || i > rows - 1 || j < 0 || j > cols - 1 || board[i][j] !== word[index]) {
            return false
        }
        if (index == word.length - 1) return true
        let temp = board[i][j]
        board[i][j] = '-'
        let res = dfs(i - 1, j, board, word, index + 1) || dfs(i + 1, j, board, word, index + 1) || dfs(i, j - 1, board, word, index + 1) || dfs(i, j + 1, board, word, index + 1)
        board[i][j] = temp
        return res

    }

    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (dfs(i, j, board, word, 0)) {
                return true
            }
        }
    }
    return false
};
```

#### [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

**由于涉及到移动所以遍历二维数组找出个数不可行**

**通过深度优先搜索（DFS）进行解题**

```js
/**
 * @param {number} m
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
var movingCount = function (m, n, k) {
    let checkValidation = function (m, n) {
        // console.log(m, n)
        let sum = 0
        let a = m
        let b = n
        while (a) {
            sum += a % 10
            a = Math.floor(a / 10)
        }
        while (b) {
            sum += b % 10
            b = Math.floor(b / 10)
        }
        // console.log(sum)
        return sum <= k

    }
    let dfs = function (i, j, grid) {
        if (i < 0 || i > m - 1 || j < 0 || j > n - 1 || !checkValidation(i, j) || grid[i][j] == 0) {
            return 0
        }
        grid[i][j] = 0
        return 1 + dfs(i + 1, j, grid) + dfs(i, j + 1, grid)

    }

    let grid = Array.from(Array(m), () => Array(n))
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            grid[i][j] = 1
            return dfs(i, j, grid)
        }
    }
};
```

