# $$\color{Red}{二叉树的中序遍历【leetcode94 中序遍历】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

 

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

 

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

### 解析

#### 递归算法

递归很简单，前序遍历就是把当前节点放入答案，然后递归左节点，然后递归右节点。

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
    
    static List<Integer> ans = new ArrayList<>();
    static void dfs(TreeNode x) {
        if (x == null) return;
        dfs(x.left);
        ans.add(x.val);
        dfs(x.right);
    }

    public List<Integer> inorderTraversal(TreeNode root) {
        ans.clear();
        dfs(root);
        return ans;
    }
}
```



### 迭代算法

能理解并做出递归算法，其实迭代算法就差不多了。

递归算法相当于通过函数的递归调用，强制调用了方法栈，靠压栈完成了栈底是根节点，然后递归到第一个左叶子节点，然后完成 `dfs` 后，通过方法栈弹出，能操作叶子节点的父节点。

为了能通过迭代算法完成如上操作，完全可以手动模拟一个栈代替方法栈，每当递归到深层的一个节点就压入栈内，然后让枚举节点化为当前节点的右节点，以达成左中右的遍历。

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

    static LinkedList<TreeNode> stk = new LinkedList<>();
    
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        while (root != null || !stk.isEmpty()) {
            while (root != null) {
                stk.push(root);
                root = root.left;
            }
            if (!stk.isEmpty()) {
                root = stk.pop();
                ans.add(root.val);
                root = root.right;
            }
        }
        return ans;
    }
}
```



### 这里有一个很厉害的染色法解决一次性学会迭代法

官方题解中介绍了三种方法来完成树的中序遍历，包括：

- 递归
- 借助栈的迭代方法
- 莫里斯遍历

在树的深度优先遍历中（包括前序、中序、后序遍历），递归方法最为直观易懂，但考虑到效率，我们通常不推荐使用递归。

栈迭代方法虽然提高了效率，但其嵌套循环却非常烧脑，不易理解，容易造成 “一看就懂，一写就废” 的窘况。而且对于不同的遍历顺序（前序、中序、后序），循环结构差异很大，更增加了记忆负担。

因此，我在这里介绍一种 “颜色标记法” （瞎起的名字……），兼具栈迭代方法的高效，又像递归方法一样简洁易懂，更重要的是，这种方法对于前序、中序、后序遍历，能够写出完全一致的代码。

其核心思想如下：

使用颜色标记节点的状态，新节点为白色，已访问的节点为灰色。

如果遇到的节点为白色，则将其标记为灰色，然后将其右子节点、自身、左子节点依次入栈。

如果遇到的节点为灰色，则将节点的值输出。

使用这种方法实现的中序遍历如下：

```java
class Solution {
    static class Node {
        TreeNode node; 
        int color;
        
        public Node (TreeNode node, int color) {
            this.node = node;
            this.color = color;
        }
    }

    public List<Integer> inorderTraversal(TreeNode root) {
        LinkedList<Node> stk = new LinkedList<>();
        List<Integer> ans = new ArrayList<>();
        stk.push(new Node(root, 0));
        while (!stk.isEmpty()) {
            Node cur = stk.pop();
            if (cur.node == null) continue;
            if (cur.color == 0) {
                stk.push(new Node(cur.node.right, 0));
                stk.push(new Node(cur.node, 1));
                stk.push(new Node(cur.node.left, 0));
            } else {
                ans.add(cur.node.val);
            }
        }    
        return ans;
    }
}
```



