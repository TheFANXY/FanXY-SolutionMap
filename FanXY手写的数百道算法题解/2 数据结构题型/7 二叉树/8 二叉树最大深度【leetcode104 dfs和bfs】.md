# $$\color{Red}{二叉树最大深度【leetcode104 dfs和bfs】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

 

```
输入：root = [3,9,20,null,null,15,7]
输出：3
```

**示例 2：**

```
输入：root = [1,null,2]
输出：2
```

 

**提示：**

- 树中节点的数量在 `[0, 10^4]` 区间内。
- `-100 <= Node.val <= 100`



### 解析

这道题直觉上可以使用 `bfs` 进行层序遍历，维护一个层数的变量。

也可使用 `dfs` 要么传递一个参数，不断更新当前层数，然后更新答案，但是也可直接利用递归的性质，从底部返回到根部 `dfs` 的深度。这么更简单，还不需要设置传递参数的逻辑。

#### `bfs`

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
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        LinkedList<TreeNode> q = new LinkedList<>();
        q.add(root);
        int deep = 0;
        while (!q.isEmpty()) {
            int len = q.size();
            deep ++;
            while (len -- > 0) {
                TreeNode t = q.remove();
                if (t.left != null) q.add(t.left);
                if (t.right != null) q.add(t.right);
            } 
        }
        return deep;
    }
}
```



#### `dfs`

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```



