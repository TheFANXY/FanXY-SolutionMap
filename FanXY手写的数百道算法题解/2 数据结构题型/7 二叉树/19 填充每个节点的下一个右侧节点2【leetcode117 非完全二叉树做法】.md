# $$\color{Red}{填充每个节点的下一个右侧节点2【leetcode117 非完全二叉树做法】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个二叉树：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL` 。

初始状态下，所有 next 指针都被设置为 `NULL` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

```
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化输出按层序遍历顺序（由 next 指针连接），'#' 表示每层的末尾。
```

**示例 2：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中的节点数在范围 `[0, 6000]` 内
- `-100 <= Node.val <= 100`

**进阶：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序的隐式栈空间不计入额外空间复杂度。



### 解析

这道题就不能直接根据完全二叉树的方法做了。

因为并不知道当前节点应该指向的下个节点是不是父节点的下一个节点的子节点，甚至有可能相隔很远，靠 `if` 这样的条件判断，无法枚举所有情况。

**那么难不成需要真的靠宽搜开队列？ 这岂不是违背了题目要求的常数额外空间？**

其实根据上一题的思路，我们每次通过一层构造下一层的节点的 `next` 指针。

所以我们遍历上一层的节点，不需要开空间，而为了记忆下一层的节点，只需要开两个常数空间，维护一个链表，让子节点不断维护在链表中，通过 `next` 指针维护关系，这样当维护下一层的 `next` 之后，直接遍历下一层节点即可。

下一层的第一个节点就是之前维护的链表的第一个节点【除去虚拟头节点】

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        Node p = root;
        if (p == null) return root;
        while (p != null) {
            Node head = new Node(-1);
            Node tail = head;
            for (Node q = p; q != null; q = q.next) {
                if (q.left != null) tail = tail.next = q.left;
                if (q.right != null) tail = tail.next = q.right;
            }
            p = head.next;
        }
        return root;
    }
}
```

