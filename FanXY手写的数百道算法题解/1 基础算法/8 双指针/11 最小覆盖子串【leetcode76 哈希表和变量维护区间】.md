# <font color="bb000">最小覆盖子串(困难)【leetcode76 哈希表和变量维护区间】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

 

**注意：**

- 对于 `t` 中重复字符，我们寻找的子字符串中该字符数量必须不少于 `t` 中该字符数量。
- 如果 `s` 中存在这样的子串，我们保证它是唯一的答案。

 

**示例 1：**

```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。
```

**示例 2：**

```
输入：s = "a", t = "a"
输出："a"
解释：整个字符串 s 是最小覆盖子串。
```

**示例 3:**

```
输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。
```

 

**提示：**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 10^5`
- `s` 和 `t` 由英文字母组成

 

**进阶：**你能设计一个在 `o(m+n)` 时间内解决此问题的算法吗？





### 解析

这道题初看应该直觉都是双指针，因为要求的是字符串一段包含另外一个字符串的整体，但是可以无序。

那么我们给两个字符串命名 `s => 源串 ， t => 模板串`，而维护的区间为一段字符串，满足不冗余出现多余的字符，目前 `i` 为右端点，`j` 可以取最大【尽可能靠右，即删除 `j` 位置字符仍满足上述不冗余的情况】 的最短字符串。

但是其实都能想到，肯定 `i` 不断右移，`j` 随着 `i` 的移动，其实有两种可能。

1. `j` 对应位置的字符，根本不是模板串包含的字符，那么 `j` 位置字符冗余，可以右移。
2. `j` 对应位置的字符，是模板串包含的字符，但是，目前区间 `[j, i]` 内部的对应 `j` 位置字符数量，要比模板串内部这个字符数量还多，那其实 `j` 位置字符仍冗余，还是可以右移。



那么难点就体现在如何能快速获取 **动态变化的区间内部** 的`字符` 到底都有哪些，数量是多少？和模板串比，同种字符的数量是多还是少？

这个也是我最开始做这道题陷入的瓶颈，我最开始的想法是，`靠一个哈希表能存区间内部的字符对应数量`，而为了去维护双指针区间，该怎么拿区间内部的字符比对到底是多是少？

很蠢的是我直接通过每次移动，遍历一下较短的模板串，和哈希表进行比对，进行统计。结果超时。

**后面意识到了，其实都是统计数量，为什么不给原串也写一个哈希表，直接同种字符，比同一个 `key` 的数量关系不就 `O(1)` 了！**

至此这道题，结束。

其实后面看过 `y总` 的代码，发现其实一个哈希表就可以，直接做一个对模板串的哈希表，然后区间的 `i` 移动，通过减法运算，那么不在原串的自然变成了负数。

对于总数量的部分，其实可以开一个变量存区间符合条件的总数量，利用两个哈希表进行比对即可，这样更新答案完全就不需要任何比较和遍历操作，只需要靠维护变量，拿变量的值判断是不是和模板串长度一样即可。

```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap <Character, Integer> hs = new HashMap<>();
        HashMap <Character, Integer> ht = new HashMap<>();

        for (int i = 0; i < t.length(); i ++) {
            char ci = t.charAt(i);
            ht.put(ci, ht.getOrDefault(ci, 0) + 1);
        }

        String res = "";
        for (int i = 0, j = 0, cnt = 0; i < s.length(); i ++) {
            char ci = s.charAt(i);
            hs.put(ci, hs.getOrDefault(ci, 0) + 1);
            // 判断 i 位置 是否是有效的一个字符
            if (ht.containsKey(ci) && hs.get(ci) <= ht.get(ci)) cnt ++;

            // 双指针维护区间
            while (j <= i && (!ht.containsKey(s.charAt(j)) || hs.get(s.charAt(j)) > ht.get(s.charAt(j)))) {
                hs.put(s.charAt(j), hs.get(s.charAt(j ++)) - 1);
            }
            // 当满足全部有效的情况下进行答案维护
            if (cnt == t.length() && (res == "" || res.length() > i - j + 1)) 
                res = s.substring(j, i + 1);
        }
        return res;
    }
}
```

单个哈希表就可以完成了。

```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character, Integer> mt = new HashMap<>();

        int n = s.length();
        int m = t.length();
        int needMatch = 0;
        String res = "";
        // 未匹配为正数
        for (char ct : t.toCharArray()) {
            if (!mt.containsKey(ct)) needMatch ++;
            mt.put(ct, mt.getOrDefault(ct, 0) + 1);
        }
        for (int L = 0, R = 0; R < n; R ++) {
            char cs = s.charAt(R);
            mt.put(cs, mt.getOrDefault(cs, 0) - 1);
            if (mt.get(cs) == 0) needMatch --;
            while (L < R && mt.get(s.charAt(L)) < 0) {
                if (s.charAt(L) == -1) needMatch --;
                mt.put(s.charAt(L), mt.getOrDefault(s.charAt(L), 0) + 1);
                L ++;
            }
            if (needMatch == 0 && res.length() == 0 || res.length() > R - L + 1) {
                res = s.substring(L, R + 1);
            }
        }
        return res;
    }
}
```

