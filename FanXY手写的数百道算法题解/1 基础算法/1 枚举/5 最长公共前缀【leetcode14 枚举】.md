# <font color='bb000'>最长公共前缀【leetcode14 枚举】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

 

**示例 1：**

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

**示例 2：**

```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

 

**提示：**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` 仅由小写英文字母组成



### 解析 

很简单的思路，双层循环，从第一位开始判断到最后一位【不越界的情况】，然后判断是不是全部字符串该位都相等，这里开`trie`树并不是好做法，空间复杂度太高。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) return "";
        if (strs.length == 1) return strs[0];

        StringBuilder ans = new StringBuilder("");

        for (int i = 0; i < strs[0].length(); i ++) {
            for (String s : strs) 
                if (i >= s.length() || s.charAt(i) != strs[0].charAt(i)) return ans.toString();
            ans.append(strs[0].charAt(i));
        }
        return ans.toString();
    }
}
```

