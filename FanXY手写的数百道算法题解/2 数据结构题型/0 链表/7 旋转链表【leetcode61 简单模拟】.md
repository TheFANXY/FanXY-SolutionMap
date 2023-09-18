# <font color='bb000'>旋转链表【leetcode61 简单模拟】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/)  

给你一个链表的头节点 `head` ，旋转链表，将链表每个节点向右移动 `k` 个位置。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)

```
输入：head = [0,1,2], k = 4
输出：[2,0,1]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 500]` 内
- `-100 <= Node.val <= 100`
- `0 <= k <= 2 * 10^9`



### 解析

这道题直观上看，就是模拟的一个过程，因为仔细想，旋转之后相当于截断后面一段，顺序不变，只是尾节点指向之前的头节点，截断部分的头节点，变成现在的头节点，而截断部分的前一个节点需要指向空。

**但是有一个很关键的边界需要考虑**，就是当 `k >= n` 的情况下，其实相当于变回原链表又重新旋转，所以很明显的一个推断就是需要 `k` 对 `n` 取模。



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null) return null;
        int n = 0;
        ListNode tail = new ListNode();
        for (ListNode p = head; p != null; p = p.next) {
            n++;
            tail = p;
        }
        k %= n;
        if (k == 0) return head;

        ListNode sentinel = head;
        for (int i = 0; i < n - k - 1; i ++) sentinel = sentinel.next;
        tail.next = head;
        head = sentinel.next;
        sentinel.next = null;
        return head;
    }
}
```





