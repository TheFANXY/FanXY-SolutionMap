# <font color="bb000">删除链表中的重复节点2【leetocede81 双指针判区间】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个已排序的链表的头 `head` ， *删除原始链表中所有重复数字的节点，只留下不同的数字* 。返回 *已排序的链表* 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)

```
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)

```
输入：head = [1,1,1,2,3]
输出：[2,3]
```

 

**提示：**

- 链表中节点数目在范围 `[0, 300]` 内
- `-100 <= Node.val <= 100`
- 题目数据保证链表已经按升序 **排列**



### 解析

删除重复元素，并且已经相同元素放到一起，这个明着告诉你就是双指针，不过链表题几乎都是指针操作。

最开始没仔细看题，因为是把重复的只留一个，2分钟把简易双指针打出来。

感慨很简单，直接两个指针，一个 `cur` 一个 `sentinel` 开局都指向 `head` ，当相等的情况 `while` 循环下 `sentinel` 一直后移，然后 cur 最终指向 `sentinel` 然后把 `cur` 指向 `sentinel` 进入下一次循环。

直接提交了，发现居然是全部删除，那只能在此基础上，留一个指针保存 `cur` 的前一个位置。按我的习惯命名为 `pre`。

那最外层直接用 `pre` 的 `next` 是否为空作为循环条件，如果没有下一个节点，说明已经没有节点需要判重了，已经遍历到尾巴了。

**然后和上面大同小异** ，仍是两个指针，一个 `cur` 一个 `sentinel` 开局都指向 `head` ，当相等的情况 `while` 循环下 `sentinel` 一直后移。

但是这道题是全部删除，根据 `while` 条件，如果只有一个相等的节点，那么 `cur.next` 一定是 `sentinel` ，就不需要进行指针操作，直接让 `pre` 向后移动一个，判断下一个节点有没有相等节点即可。

如果 `cur.next != sentinel` ，很显然此时说明出现了相等的节点，直接 `pre.next = sentinel` ，跳过这一段的相等的所有节点，当然，此时 `pre` 无需移动，因为下一个位置就是新节点，直接无需移动，迭代进入循环判断下一个位置即可。

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
        ListNode pre = dummy;

        while (pre.next != null){
            ListNode cur = pre.next, sentinel = pre.next;
            // 双指针划分相等节点区间
            while (sentinel != null && cur.val == sentinel.val) sentinel = sentinel.next;
            // 当存在相等节点 跳过这一段节点 pre 节点无需位置变化 下个循环直接判断新节点
            if (sentinel != cur.next) pre.next = sentinel;
            // 无相等节点 移位 pre 判断下一个节点
            else pre = pre.next;
        } 
        return dummy.next;
    }
}
```

