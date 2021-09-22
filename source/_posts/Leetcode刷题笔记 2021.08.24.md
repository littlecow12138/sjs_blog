---
title: Leetcode刷题笔记 2021.08.24
tag: ['leetcode', '树的遍历']
categories: 
- leetcode
---
# Leetcode刷题笔记 2021.08.24

## 搜索与回溯算法专题

### 基础知识回顾

#### 树的遍历

**深度优先遍历（DFS）：**

- **先序遍历：**先访问节点本身，再访问左侧子节点，最后是右侧子节点
- **中序遍历：**先访问左侧子节点，再访问节点本身，最后事右侧子节点
- **后序遍历：**先访问左侧子节点，在访问右侧子节点，最后访问节点本身

**广度优先遍历（BFS）：**

通过队列的辅助，每次去除队列头部节点访问该节点并将左右子节点分别压入队列

#### [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

例如:
给定二叉树: `[3,9,20,null,null,15,7]`，返回：`[3,9,20,15,7]`

**算法：广度优先搜索（BFS）**

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
 * @return {number[]}
 */
var levelOrder = function (root) {
    if(!root) return []
    let queue = [root]
    let res = []
    while (queue.length) {
        let new_node = queue.shift()
        res.push(new_node.val)
		// 通过将根节点入队，再获取最前面的根节点出队，将子节点入队实现广度优先搜索（BFS）
        new_node.left && queue.push(new_node.left)
        new_node.right && queue.push(new_node.right)
    }
    return res
};
```

#### [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

**通过临时数组tmp存储，并且通过倒序方式实现初始队列长度的遍历**

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
 * @return {number[][]}
 */
var levelOrder = function (root) {
    if (!root) return []
    let queue = [root]
    let res = []

    while (queue.length) {
        let tmp = []
        for (let i = queue.length - 1; i >= 0; i--) {
            let new_node = queue.shift()
            tmp.push(new_node.val)

            new_node.left && queue.push(new_node.left)
            new_node.right && queue.push(new_node.right)
        }
        res.push(tmp)
    }
    return res
};
```

#### [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

**通过数组reverse()完成逆序操作**

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
 * @return {number[][]}
 */
var levelOrder = function (root) {
    if (!root) return []
    let queue = [root]
    let res = []
    let re = true

    while (queue.length) {
        let tmp = []
        for (let i = queue.length - 1; i >= 0; i--) {
            let new_node = queue.shift()
            tmp.push(new_node.val)

            new_node.left && queue.push(new_node.left)
            new_node.right && queue.push(new_node.right)
        }
        if (re) {
            res.push(tmp)
            re = !re
        } else {
            res.push(tmp.reverse())
            re = !re
        }

    }
    return res
};
```

#### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

**通过先序遍历进行对比**

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} A
 * @param {TreeNode} B
 * @return {boolean}
 */
var flag = true

var isSubStructure = function (A, B) {
    if (!B || !A) return false
    let n = A
    let stack = [n]
    while (stack.length) {
        let n = stack.pop()
        if (n.val == B.val) {
            flag = true
            dfs(n, B)
            if (flag) return true
        }
        n.right && stack.push(n.right)
        n.left && stack.push(n.left)
    }
    return false
};

var dfs = function (A, B) {
    if (!A || !B) return false
    // console.log(A.val, A.left, B.val, B.left, B.left && !A.left)
    if (A.val !== B.val) return flag = false
    if (B.left && !A.left) return flag = false
    if (B.right && !A.right) return flag = false
    if (A.left && B.left)
        dfs(A.left, B.left)
    if (A.right && B.right)
        dfs(A.right, B.right)
}
```

#### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

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
 * @return {TreeNode}
 */
var mirrorTree = function (root) {
    if (root === null) {
        return null;
    }
    const left = mirrorTree(root.left);
    const right = mirrorTree(root.right);
    root.left = right;
    root.right = left;
    return root;
}
```

