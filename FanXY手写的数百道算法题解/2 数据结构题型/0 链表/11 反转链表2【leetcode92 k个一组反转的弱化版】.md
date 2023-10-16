# <font color="bb000">反转链表2【leetcode92 k个一组反转的弱化版】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

**示例 2：**

```
输入：head = [5], left = 1, right = 1
输出：[5]
```

 

**提示：**

- 链表中节点数目为 `n`
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

 

**进阶：** 你可以使用一趟扫描完成反转吗？





### 解析

给定范围的节点进行反转，那么就是反转链表的升级版，反转链表经典必须存反转部分的前一个节点(这里记为 `pre`)，即需要锁定 `left` 节点前一个节点 因为反转之后要进行前后连接。

其次反转多少个节点，可以直接根据 `right - left` 得出反转的边数，这个很简单。

问题在于反转之后，需要进行 `pre` 和反转部分，以及反转部分后面的剩余链表节点的三部分拼接。

### [我的题解`for`合并K个排序链表【leetcode23 堆替代归并】](https://www.acwing.com/solution/content/201013/)

这个题比上面的更简单，因为不需要进行 `pre` 节点的移动，以完成下一段反转操作。

所以不需要额外开一个变量存 `pre.next` ，直接进行前后拼接即可。

这部分想看详细拼接分析可以看上面的题解链接或者下面的代码。



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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null || head.next == null) return head;
        ListNode dummy = new ListNode(0, head);
        ListNode pre = dummy;

        // 反转链表经典操作 锁定头节点前一个节点 因为反转之后要进行前后连接
        for (int k = 0; k < left - 1; k ++) pre = pre.next;

        // 进行反转链表操作
        ListNode cur = pre.next;
        ListNode sentinel = cur.next;
        for (int i = 0; i < right - left; i ++) {
            ListNode temp = sentinel.next;
            sentinel.next = cur;
            cur = sentinel;
            sentinel = temp;
        }

        // 反转部分和未反转部分进行拼接
        pre.next.next = sentinel;
        pre.next = cur;
        return dummy.next;
    }
}
```

