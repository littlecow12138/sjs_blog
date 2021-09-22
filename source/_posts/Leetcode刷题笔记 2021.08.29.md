---

title: Leetcode刷题笔记 2021.08.29

tag: ['leetcode', '动态规划']

categories: 

- leetcode

- dp

---

# Leetcode刷题笔记 2021.08.29

## 动态规划

#### [剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

**动态规划：**

- **状态定义：dp[i]表示到第i个字符有几种不同的翻译**
- **状态转移方程：**
  - **0 <= Number(num[j] + num[i]) <= 25: dp[i] = dp[i-1] + dp[i-2]**
  - **Number(num[j] + num[i]) > 25 dp[i] = dp[i-1]**
- **初始化：dp[0] = 1**

***PS: js中大于小于符号的连用问题。例如（1001<4001<1010）***

***在js的逻辑中，程序会按运算符优先级，先计算左边的，左边的返回应该是bool值，拿这个bool值再结合右边的计算***

- ***1001<=4001 返回true（即1）***
- ***1 < 1010 返回true***

```js
/**
 * @param {number} num
 * @return {number}
 */
var translateNum = function (num) {
    let dp = []
    num = num.toString()
    dp[0] = 1
    // console.log(dp[0])
    for (let i = 1; i < num.length; i++) {
        if (Number(num[i - 1] + num[i]) >= 10 && Number(num[i - 1] + num[i]) <= 25) {
            // console.log(Number(num[i - 1] + num[i]) <= 25)
            if (i - 2 >= 0) {
                dp[i] = dp[i - 1] + dp[i - 2]
            } else {
                dp[i] = 2
            }
        } else {
            dp[i] = dp[i - 1]
        }
    }
    return dp[num.length - 1]
};
```

#### [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

动态规划：

- 状态定义：dp[i]表示到第i个字符的字串的无重复字符字串的最大长度
- 状态转移方程：设s为s[j]左侧最近的相同字符
  - i<0：dp[i-1] + 1
  - dp[i-1]<i-s：dp[i-1] + 1
  - dp[i-1]>=i-s：dp[i] = i - s
- 初始化：dp[0] = 1

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function (s) {
    if (s == '') return 0
    let dp = []
    dp[0] = 1
    let res = 1
    for (let i = 1; i < s.length; i++) {
        let b = i - 1
        while (b >= 0) {
            if (s[b] == s[i]) {
                break
            }
            b--
        }
        if (b < 0 || i - b > dp[i - 1]) {
            dp[i] = dp[i - 1] + 1
            if (res < dp[i]) {
                res = dp[i]
            }
            continue
        }
        if (i - b <= dp[i - 1]) {
            dp[i] = i - b
            if (res < dp[i]) {
                res = dp[i]
            }
        }
    }
    return res
};
```

