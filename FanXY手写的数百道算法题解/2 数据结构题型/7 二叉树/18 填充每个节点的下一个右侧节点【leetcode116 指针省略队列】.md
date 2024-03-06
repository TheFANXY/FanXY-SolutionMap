# $$\color{Red}{填充每个节点的下一个右侧节点【leetcode116 指针省略队列】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个 **完美二叉树** ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。
```



**示例 2:**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点的数量在 `[0, 212 - 1]` 范围内
- `-1000 <= node.val <= 1000`

 

**进阶：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。



### 解析

这道题是一个满二叉树，其实一看题就发现应该是层序遍历了，同层从左到右应该是形成一个链表，其实开个队列 `bfs` 就能轻松解决。

但是题目要求常数空间。

这就麻烦了，其实这里有个题目隐藏的东西，我们本身存在 `next` 指针，这是和其他的二叉树不一样的地方，那么我们遍历下一层，其实只需要遍历上一层的节点，可以通过 `next` 指针不断右移，然后访问下一层只需要通过 `left` 和 `right` 两个指针进行访问建立下一层的 `next` 指针即可。

这样遍历完下一层之后，只需要变换遍历到 下一层第一个节点，开始下一层的单层向右遍历，然后建立下下层的 `next` 指针即可。

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
        if (root == null) return null;
        Node p = root;
        while (p.left != null) {
            for (Node q = p; q != null; q = q.next) {
                q.left.next = q.right;
                if (q.next != null) 
                    q.right.next = q.next.left;
            }
            p = p.left;
        }
        return root;
    }
}
```

