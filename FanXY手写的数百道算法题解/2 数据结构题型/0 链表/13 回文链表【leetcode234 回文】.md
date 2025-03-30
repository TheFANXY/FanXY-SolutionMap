# <font color="bb000">回文链表【leetcode234 回文】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
输入：head = [1,2,2,1]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
输入：head = [1,2]
输出：false
```

**提示：**

- 链表中节点数目在范围`[1, 105]` 内
- `0 <= Node.val <= 9`

**进阶：**你能否用 `O(n)` 时间复杂度和 `O(1)` 空间复杂度解决此题？



### 解析

`O(n)` 很好想到，只需要形成一个倒序的链表就行，然后从头到尾遍历这俩链表，出现不相等的情况下，说明就不是回文，如果遍历到 `null` 说明全部匹配。

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;
        ListNode t = new ListNode(head.val, null);
        ListNode p = head.next;
        while (p != null) {
            ListNode temp = new ListNode(p.val, t);
            t = temp;
            p = p.next;
        }
        while (t != null && head != null) {
            if (t.val == head.val) {
            t = t.next;
            head = head.next;
            } 
            else break;
        }
        return t == null;
    }
}
```

