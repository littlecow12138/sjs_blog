---

title: Leetcode刷题笔记 2021.08.30

tag: ['leetcode', '快慢指针']

categories: 

- leetcode

- 快慢指针

---
# Leetcode刷题笔记 2021.08.30

## 双指针（简单）

#### [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

**暴力法**

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var deleteNode = function (head, val) {
    if (head.val == val) {
        head = head.next
        return head
    }
    let pre = head
    let cur = head.next
    while (cur) {
        if (cur.val == val) {
            pre.next = cur.next
        }
        pre = cur
        cur = cur.next
    }
    return head
};
```

#### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

**快慢指针**

**让fast指针先走k个节点，然后再让fast和slow再同时走**

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var getKthFromEnd = function(head, k) {
    let fast = head
    let slow = head
    let count=k
    while(count) {
        fast = fast.next
        count--
    }
    while(fast) {
        slow = slow.next
        fast = fast.next
    }
    // console.log(fast, slow)
    return slow
};
```



