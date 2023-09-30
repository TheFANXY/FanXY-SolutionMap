# $$\color{Red}{单词搜索【leetcode79 网格搜状态路径】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
输出：true
```

**示例 3：**

![img](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
输出：false
```

 

**提示：**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` 和 `word` 仅由大小写英文字母组成

 

**进阶：**你可以使用搜索剪枝的技术来优化解决方案，使其在 `board` 更大的情况下可以更快解决问题？





### 解析

爆搜题，没什么好说的，直觉就是先遍历矩阵，找第一个满足字符和寻找字符串第一个字符相同的进行搜索。

然后为了以后按顺序一个个搜，搜索函数显然应该记录当前搜到第几个字符了，剪枝自然就用是否搜的这个字符和当前搜索模板串对应位字符是否相等判断。

搜完了最后一位就说明搜到了。

为了不开一个新的 `st` 数组来记录每一位是否已经被搜，就选择一旦一位被确定合法，就给它变成一个特殊字符，然后再剪枝判断加入这个字符判断即可。因此每当搜完一个位置的三个方向，发现都走不通，肯定是需要进行恢复现场的。

```java
class Solution {

    static int [] dx = {-1, 0, 1, 0}, dy = {0, 1, 0, -1};

    static boolean dfs(char [][] board, String word, int u, int x, int y) {
        if (board[x][y] != word.charAt(u)) return false;
        if (u == word.length() - 1) return true;
        // 预先锁定当前已匹配位置
        char temp = board[x][y];
        board[x][y] = '*';
        
        for (int i = 0; i < 4; i ++) {
            int a = x + dx[i];
            int b = y + dy[i];
            if (a < 0 || a >= board.length || b < 0 || b >= board[0].length || board[a][b] == '*') continue;
            if (dfs(board, word, u + 1, a, b)) return true;
        }
        
        board[x][y] = temp;
        return false;
    }

    public boolean exist(char[][] board, String word) {
        for (int i = 0; i < board.length; i ++) 
            for (int j = 0; j < board[0].length; j ++) {
                if (board[i][j] == word.charAt(0)) 
                    if (dfs(board, word, 0, i, j))
                        return true;
            }
        return false;
    }
}
```

