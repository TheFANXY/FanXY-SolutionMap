# <font color="bb000">删除链表中的重复节点1【leetocede81 双指针判区间】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个已排序的链表的头 `head` ， *删除所有重复的元素，使每个元素只出现一次* 。返回 *已排序的链表* 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

```
输入：head = [1,1,2]
输出：[1,2]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)

```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

 

**提示：**

- 链表中节点数目在范围 `[0, 300]` 内
- `-100 <= Node.val <= 100`
- 题目数据保证链表已经按升序 **排列**



### 解析

`leetcode` 这个题目排序是不是多少有点问题。。。。上一题就是这道题的加强版，我都做了上一题做这题不是多此一举。

做上一题的时候，看错题，最开始 2分钟就把这个第一题的做出来了，可以直接看我的代码，也可直接去我第二题题解看两道题的详细解析思路。

#### [关于这两道题我的详细解析题解](https://www.acwing.com/solution/content/206106/)

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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(-1, head);
        ListNode cur = dummy.next;
        while (cur != null) {
            ListNode sentinel = cur;
            while (sentinel != null && cur.val == sentinel.val) sentinel = sentinel.next;
            cur.next = sentinel;
            cur = sentinel;
        }
        return dummy.next;
    }
}
```







