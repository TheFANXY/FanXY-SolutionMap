$$\color{Red}{递归实现指数型枚举【leetcode78 子集】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud/archives/SolutionMap)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 



**这道题 在 `acwing` 和 `leetcode` 都出现过，但是一个是 `acm` 模式，一个是 `核心模式` 所以代码略有不同，这里就都列举一下。**

### [`acwing`对应原题位置](https://www.acwing.com/problem/content/description/94/)



给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**



### 解析

包含空集，那么其实就枚举整个数组的位置，分别把选当前数和不选当前数分为两个情况进行搜索，搜完最后一个位置加入答案即可。



#### leetcode代码

```java
class Solution {

    static List<List<Integer>> ans = new ArrayList<>();
    static List<Integer> path = new ArrayList<>();
    
    static void dfs(int n, int [] nums) {
        if (n == nums.length) {
            ans.add(new ArrayList<>(path));
            return;
        }
        path.add(nums[n]);
        dfs(n + 1, nums);
        path.remove(path.size() - 1);
        dfs(n + 1, nums);
    }

    public List<List<Integer>> subsets(int[] nums) {
        ans.clear();
        dfs(0, nums);
        return ans;
    }
}
```



#### acwing代码

```java
import java.util.*;

public class Main {
    static int n;
    static boolean st[] = new boolean [16];
    static int a[] = new int [16];
    
    public static void dfs(int x){
        if(x > n){
            for(int i=1; i<=n; i++)
                if(st[i]) System.out.print(i + " ");
            System.out.println();
            return;
        }
        st[x] = true;
        dfs(x+1);
        st[x] = false;
        dfs(x+1);
    }
    
    public static void main(String [] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        dfs(1);
    }
}
```



