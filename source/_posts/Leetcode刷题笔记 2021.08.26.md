---
title: Leetcode刷题笔记 2021.08.26
tag: ['leetcode', '动态规划']
categories: 
- leetcode
- dp
---
# Leetcode刷题笔记 2021.08.26

## 动态规划（简单）

#### [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

**通过每层存入数组，在头尾比较是否相同判断是否为对称二叉树**

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isSymmetric = function (root) {
    let queue = [root]
    while (queue.length) {
        let compare = []
        for (let i = queue.length - 1; i >= 0; i--) {
            let n = queue.shift()
            if (n !== null) {
                compare.push(n.val)
                // console.log(i)
                // console.log(n.left, n.right)
                if (!n.left) {
                    queue.push(null)
                    // console.log('left: ' + null)
                } else {
                    // console.log('left: ' + n.left.val)
                    queue.push(n.left)
                }
                if (!n.right) {
                    queue.push(null)
                    // console.log('right: ' + null)
                } else {
                    // console.log('right: ' + n.right.val)
                    queue.push(n.right)
                }
            } else {
                compare.push(null)
            }
        }
        let start = 0
        let end = compare.length - 1
        // console.log(compare)
        while (compare.length > 1 && start <= end) {
            // console.log(compare[start], compare[end])
            if (compare[start] !== compare[end]) {
                return false
            }
            start++
            end--
        }
    }
    return true
};
```

#### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

**通过递归实现，但是简单的递归如果深度过大，就会导致栈溢出，因此选择通过动态规划解决栈溢出问题**

```js
/**
 * @param {number} n
 * @return {number}
 */
var fib = function (n) {
    let n1 = 0
    let n2 = 1
    for (let i = 0; i < n; i++) {
        let sum = (n1 + n2) % 1000000007
        n1 = n2
        n2 = sum
    }
    return n1
};
```

#### [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

**斐波那契数列衍生问题**

```js
/**
 * @param {number} n
 * @return {number}
 */
var numWays = function(n) {
    let n0 = 1
    let n1 = 1
    for(let i=0; i<n; i++){
        let sum = (n0+n1) % 1000000007
        n0 = n1
        n1 = sum
    }
    return n0
};
```

#### [剑指 Offer 63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

**遍历找出最大利润（暴力法）**

```js
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
    let res = 0
    
    for (let i = 0; i < prices.length; i++) {
        for (let j = 0; j < prices.length; j++) {
            if (i >= j) {
                continue
            }
            if (prices[i] > prices[j]) {
                continue
            }
            if(prices[j] - prices[i]>res) {
                res = prices[j] - prices[i]
            }
        }
    }
    
    return res
};
```

**找出历史最低点，再计算当天和历史最低点计算的最大利润**

```js
var maxProfit = function(prices) {
    let minprice = Number.MAX_VALUE;
    let maxprofit = 0;
    for (const price of prices) {
        maxprofit = Math.max(price - minprice, maxprofit);
        minprice = Math.min(price, minprice);
    }
    return maxprofit;
};
```



