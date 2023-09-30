$$\color{Red}{递归实现组合型枚举【leetcode77 组合枚举】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud/archives/SolutionMap)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 



**这道题 在 `acwing` 和 `leetcode` 都出现过，但是一个是 `acm` 模式，一个是 `核心模式` 所以代码略有不同，这里就都列举一下。**

### [`acwing`对应原题位置](https://www.acwing.com/problem/content/description/95/)

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

 

**示例 1：**

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**示例 2：**

```
输入：n = 1, k = 1
输出：[[1]]
```

 

**提示：**

- `1 <= n <= 20`
- `1 <= k <= n`



### 解析

先是 当时做 `leetcode` 的思路

组合类型和全排列枚举的区别就是，全排列即便是相同数字的不同顺序也是不同的一组答案，但是组合类型只要数字相同就规定为一个答案，所以我们需要处理这个相同数字被多计入答案的问题。

那就是靠规定严格的字典序，来进行排序，一旦选择一个数，只能选这个数之后的数，固 `dfs` 逻辑孕育而生。

`dfs(int n, int k, int st)`

代表一共 `[1, n]` 可选，还剩 `k` 个数没选，这次选择可以从 `st` 开始选。

这里其实为了避免无意义的冗余判断，可以进行提前剪枝。

即，当 `[st, n]` 这一段所有的数的个数 `n - st + 1` 如果小于 `k` ，那就代表剩下的数字全选都无法凑够一个答案，可以直接剪枝，无需再枚举后续的搜索树。

```java 
class Solution {
    static List<List<Integer>> ans = new ArrayList<>();
    static List<Integer> path = new ArrayList<>();
    
    static void dfs(int n, int k, int st) {
        if (n - st + 1 < k) return;
        if (k == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }

        for (int i = st; i <= n; i ++) {
            path.add(i);
            dfs(n, k - 1, i + 1);
            path.remove(path.size() - 1);
        }
    }
    
    public List<List<Integer>> combine(int n, int k) {
        ans.clear();
        dfs(n, k, 1);
        return ans;
    }
}
```





### acwing版证明思路

##### 参数
 **`dfs(int u, int st)`** 表明我们当前枚举到第u个数字（已经枚举了u-1个），我们下一个数字可以从`st`开始的数字去枚举



##### 如何区分和之前的排列数字的排列型
`——》`规定一个排列顺序`——》`如字典序：那么`1 2 3`的组合就会唯一确定 不会产生其他五种顺序



##### 如何剪枝
`——》` 当我们枚举到u 代表已经枚举了`u-1`个数字，一共需要`m`个数字，下一个数字从`st`开始枚举，当`st`一直到`n`全部依次枚举补全`（n - st + 1）`个数字，加上`u - 1`的总数小于m的情况下，即不需要继续枚举
即 **`u - 1 + (n - st + 1) < m `**
即 **`u + n - st < m`**



```java
import java.util.*;
import java.io.*;

public class Main {
    static int n, m, N = 30;
    static int a[] = new int [N];
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static void dfs(int u, int st) throws IOException {
        // 优化剪枝策略
        if (u + n - st < m) return;
        if (u == m + 1) {
            for(int i=1; i<=m; i++) bw.write(a[i] + " ");
            bw.newLine();
            return;
        }
        for(int i=st; i<=n; i++) {
            a[u] = i;
            dfs(u + 1, i + 1);
          //a[u] = 0; 恢复现场 此题无需 逻辑完整性需要写
        }
    }
 
    public static void main(String [] args) throws IOException {
        String s1 [] = br.readLine().split(" ");
        n = Integer.parseInt(s1[0]);
        m = Integer.parseInt(s1[1]);
        dfs(1, 1);
        bw.close();
    }
}
```