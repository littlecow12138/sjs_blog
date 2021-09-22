---

title: Leetcode刷题笔记 2021.08.31

tag: ['leetcode', '双指针']

categories: 

- leetcode

- 双指针

---
# Leetcode刷题笔记 2021.08.31

## 双指针

**遍历，指向两个节点，将最小**

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function (l1, l2) {
    let cur1 = l1
    let cur2 = l2
    let cur3 = new ListNode()
    let head = cur3
    while (cur2 !== null && cur1 !== null) {
        if (cur1.val >= cur2.val) {
            cur3.next = cur2
            cur3 = cur3.next
            cur2 = cur2.next
        } else {
            cur3.next = cur1
            cur3 = cur3.next
            cur1 = cur1.next
        }
    }
    if(cur1) {
        cur3.next = cur1
    }
    if(cur2) {
        cur3.next = cur2
    }
    // console.log(head)
    return head.next
};
```

#### [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

**双指针**

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function (headA, headB) {
    let p1 = headA
    let p2 = headB
    while (p1 !== p2) {
        p1 = p1 == null ? headB : p1.next
        p2 = p2 == null ? headA : p2.next
    }
    return p1
};
```

