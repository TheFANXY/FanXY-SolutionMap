# <font color="bb000">分割链表【leetcode86 靠指针维护虚拟双链表】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个链表的头节点 `head` 和一个特定值 `x` ，请你对链表进行分隔，使得所有 **小于** `x` 的节点都出现在 **大于或等于** `x` 的节点之前。

你应当 **保留** 两个分区中每个节点的初始相对位置。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

```
输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]
```

**示例 2：**

```
输入：head = [2,1], x = 2
输出：[1,2]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 200]` 内
- `-100 <= Node.val <= 100`
- `-200 <= x <= 200`



### 解析

最简单的方法，肯定是创建两个新的链表，然后遍历之前的链表，把对应的值复制到一个小于 `x` 的链表，一个放到大于等于 `x` 的链表。

但是会新建大量空间，不如新建的是两个虚拟头节点，直接一边遍历一边把原链表节点放入虚拟链表中，相当于原地算法，然后插入虚拟链表的过程，其实就是改变原链表节点指向的过程。

最后让大于等于 `x` 的虚拟链表尾部指向 `null` ，然后小于 `x` 的链表尾部节点指向 大于 `x` 的虚拟链表的头节点，然后返回 小于 `x` 的虚拟链表头节点，大功告成。



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
    public ListNode partition(ListNode head, int x) {
        ListNode small = new ListNode(0);
        ListNode big = new ListNode(0);
        ListNode cur = head;
        ListNode sCur = small;
        ListNode bCur = big;

        // 双指针原地将原链表分割两个链表
        while (cur != null) {
            if (cur.val < x) {
                sCur.next = cur;
                sCur = sCur.next;
            } else {
                bCur.next = cur;
                bCur = bCur.next;
            }
            cur = cur.next;
        }

        // 链表合并
        bCur.next = null;
        sCur.next = big.next;

        return small.next;
    }
}
```

