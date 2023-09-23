# <font color='red'>不同路径2【leetcode63 有障碍版】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

### 题目说明

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

```
输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

 

**提示：**

- `m == obstacleGrid.length`
- `n == obstacleGrid[i].length`
- `1 <= m, n <= 100`
- `obstacleGrid[i][j]` 为 `0` 或 `1`



### 解析

### [上一题：最大连续子数组和【leetcode53最大连续和】](https://www.acwing.com/solution/content/202579/)



有了障碍物版本，有一个很重要的边界点，**如果做法不同于我的:选择给起点进行初始化，然后均更新。**

选择了初始化第一行和第一列，即 `i == 1 || j == 1` 初始化 `f[i][j] = 1`，然后选择如果出现障碍就为 `0`。

这个做法是有误的，如第一行，如果第三个位置出现障碍，理论上后续的所有的位置，都应该是 `0`，因为只能通过向右才能走到。

所以不如使用初始化起点，剩下的都靠递推，然后只是把出现障碍物的情况省却即可。【即有障碍物的递归公式值为`0`】

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] a) {
        int n = a.length;
        int m = a[0].length;

        int [][] f = new int [n][m];
        for (int i = 0; i < n; i ++)
            for (int j = 0; j < m; j ++)
                if (a[i][j] == 0) {
                    if (i == 0 && j == 0) f[i][j] = 1;
                    else {
                        if (i > 0) f[i][j] += f[i-1][j];
                        if (j > 0) f[i][j] += f[i][j-1];
                    }
                }

        return f[n-1][m-1];                
    }
}
```

