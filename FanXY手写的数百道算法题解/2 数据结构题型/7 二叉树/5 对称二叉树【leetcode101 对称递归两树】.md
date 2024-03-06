# $$\color{Red}{对称二叉树【leetcode101 对称递归两树】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

 

**提示：**

- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

 

**进阶：**你可以运用递归和迭代两种方法解决这个问题吗？



### 解析

#### 递归

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    static boolean dfs(TreeNode p, TreeNode q) {
        if (p == null && q == null) return true;
        if (p == null || q == null || p.val != q.val) return false;
        return dfs(p.left, q.right) && dfs(p.right, q.left);
    }

    public boolean isSymmetric(TreeNode root) {
        if (root == null || root.left == null && root.right == null) return true;
        return dfs(root.left, root.right);
    }
}
```



#### 迭代

需要用栈来达成迭代的回溯，同时对左子树和右子树进行相反方向的迭代遍历节点。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public boolean isSymmetric(TreeNode root) {
        if (root == null || root.left == null && root.right == null) return true;
        LinkedList <TreeNode> l_stk = new LinkedList<>(), r_stk = new LinkedList<>();
        TreeNode l = root.left, r = root.right;
        while (l != null || r != null || l_stk.size() > 0) {
            while (l != null && r != null) {
                if (l.val != r.val) return false;
                l_stk.push(l);
                r_stk.push(r);
                l = l.left;
                r = r.right;
            }
            if (l != null || r != null) return false;
            l = l_stk.pop();
            r = r_stk.pop();
            if (l.val != r.val) return false;
            l = l.right;
            r = r.left;
        }
        return true;
    }
}
```



