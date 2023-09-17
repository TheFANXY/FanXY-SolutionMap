# <font color="bb000">螺旋矩阵2【leetcode59 生成螺旋矩阵】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 





### 解析

这道题和前面不同的是，这次是自己根据给出的长度，自己生成，区别就是换成自己赋值每个位置，返回自己 new 的二维数组。

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int [][] ans = new int [n][n];
        boolean [][] st = new boolean [n][n];
        int [] dx = {-1, 0, 1, 0}, dy = {0, 1, 0, -1};
        int offset = 1, cnt = 1;
        int cur_x = 0, cur_y = 0;

        while (cnt <= n * n) {
            ans[cur_x][cur_y] = cnt++;
            st[cur_x][cur_y] = true;
            int ne_x = cur_x + dx[offset], ne_y = cur_y + dy[offset];

            if (ne_x < 0 || ne_x >= n || ne_y < 0 || ne_y >= n || st[ne_x][ne_y]) {
                offset = (offset + 1) % 4;
                ne_x = cur_x + dx[offset];
                ne_y = cur_y + dy[offset];
            }
            cur_x = ne_x;
            cur_y = ne_y;    
        }
        return ans;
    }
}
```

