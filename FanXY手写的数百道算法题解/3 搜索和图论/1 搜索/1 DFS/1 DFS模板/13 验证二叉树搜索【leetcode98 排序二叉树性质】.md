# $$\color{Red}{验证二叉树搜索【leetcode98 中序遍历】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
输入：root = [2,1,3]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

```
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

 

**提示：**

- 树中节点数目范围在`[1, 10^4]` 内
- `-2^31 <= Node.val <= 2^31 - 1`



###  解析

排序二叉树的性质，左子树的任何一点均小于根，右子树任何一点均大于根。

故由此性质可以进行二叉树的前序遍历，递归过程传递维护一段最大值和最小值，代表后面的节点应当满足的范围，每当遍历到一个节点，先进行非空判断，如果为空可以直接返回 `true` ，不为空，判断是否满足最大最小值区间内的范围，不满足代表直接非法，可以返回 `false`。

在递归遍历过程中，如果是要遍历左节点，那么应当使得维护的最大值，直接代替为当前遍历节点的值进行左节点的遍历【因为先进行过非法判断，如果不非法，代表当前节点的值，一定合法，那么遍历左节点，它的徒孙节点们应当满足一定要小于根节点的值】

而如果是要遍历右节点，那么应当使得维护的最小值，直接代替为当前遍历节点的值进行右节点的遍历【因为先进行过非法判断，如果不非法，代表当前节点的值，一定合法，那么遍历右节点，它的徒孙节点们应当满足一定要大于根节点的值】

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

    static boolean dfs(TreeNode root, long min, long max) {
        if (root == null) return true;
        if (root.val <= min || root.val >= max) return false;
        return dfs(root.left, min, root.val) && dfs(root.right, root.val, max);
    }

    public boolean isValidBST(TreeNode root) {
        return dfs(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
}
```

