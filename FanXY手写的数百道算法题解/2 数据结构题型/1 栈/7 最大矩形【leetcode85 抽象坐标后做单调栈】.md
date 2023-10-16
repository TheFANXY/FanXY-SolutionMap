# <font color="bb000">最大矩形【leetcode85 抽象坐标后做单调栈】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个仅包含 `0` 和 `1` 、大小为 `rows x cols` 的二维二进制矩阵，找出只包含 `1` 的最大矩形，并返回其面积。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg)

```
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：6
解释：最大矩形如上图所示。
```

**示例 2：**

```
输入：matrix = []
输出：0
```

**示例 3：**

```
输入：matrix = [["0"]]
输出：0
```

**示例 4：**

```
输入：matrix = [["1"]]
输出：1
```

**示例 5：**

```
输入：matrix = [["0","0"]]
输出：0
```

 

**提示：**

- `rows == matrix.length`
- `cols == matrix[0].length`
- `1 <= row, cols <= 200`
- `matrix[i][j]` 为 `'0'` 或 `'1'`



### 解析

### 前置题型: [我的【柱状图中最大的矩形】题解](https://www.acwing.com/file_system/file/content/whole/index/content/10154731/)

这道题如果没有做过上面这题，可能会下意识想着可能是 `dfs` 染色的思路。但是为了保持矩形，就会很麻烦，需要设计多种的条件判断，维持搜索的图形符合矩形。

但是做过上面这道题，提供了一个完美的思路，即抽象成柱状图进行矩形面积的计算。

但是上一题的思路有一个问题是，关于这个高度，是描述从 `x轴` 为起点，延申出来的高度。但是如果是在一个二维数组里面，这个高度的起点是任意的，不能保证起点一定来源于四个边任意一个边。

但是非起点来源于四个边，仍能形成一个矩形，这个矩形按 `两次单调栈的思路`，就枚举不到。

那么很简单，我们设定一条边的作为枚举的 `x轴` ，然后遍历同方向的所有边作为 `x轴`，然后与它垂直的边作为 `y轴`，然后对每个枚举的坐标系进行两次单调栈的做法。

即能枚举所有的情况。

显然最省事就是常规遍历二维矩阵，每一行行作为枚举的 `x轴`， 而第 0 列始终作为 `y轴` 即可，同时高度的计算可以利用动态规划，一次累积算出。

```java
class Solution {
    public static int largeRectangleArea(int [] h) {
        int n = h.length;
        int [] left = new int[n], right = new int[n];
        LinkedList <Integer> stk = new LinkedList<>();
        // 单调栈算左边第一个小于该高度的位置
        for (int i = 0; i < n; i ++) {
            while (!stk.isEmpty() && h[stk.peek()] >= h[i]) stk.pop();
            if (stk.isEmpty()) left[i] = -1;
            else left[i] = stk.peek(); 
            stk.push(i);
        }

        stk.clear();
        // 单调栈算右边第一个小于该高度的位置
        for (int i = h.length - 1; i >= 0; i --) {
            while (!stk.isEmpty() && h[stk.peek()] >= h[i]) stk.pop();
            if (stk.isEmpty()) right[i] = n;
            else right[i] = stk.peek();
            stk.push(i);
        }
        int res = 0;
        for (int i = 0; i < n; i ++) res = Math.max(res, (right[i] - left[i] - 1) * h[i]);
        return res;
    }

    public int maximalRectangle(char[][] matrix) {
        int n = matrix.length, m = matrix[0].length;
        int h[][] = new int [n][m];
        
        // 初始化高度矩阵
        for (int i = 0; i < n; i ++)
            for (int j = 0; j < m; j ++) {
                if (matrix[i][j] == '1') {
                    if (i == 0) h[i][j] = 1;
                    else h[i][j] = h[i - 1][j] + 1;
                }
            }
        int res = 0;
        // 枚举每一行作为 x 轴起点 获取最大矩形面积
        for (int i = 0; i < n; i ++) res = Math.max(res, largeRectangleArea(h[i]));
        return res;
    }
}
```





