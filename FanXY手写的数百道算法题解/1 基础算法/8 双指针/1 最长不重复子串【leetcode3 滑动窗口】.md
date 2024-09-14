# <font color='bb000'>1 无重复字符的最长子串 leetcode3</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 


给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

 

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

 

**提示：**

- `0 <= s.length <= 5 * 10^4`
- `s` 由英文字母、数字、符号和空格组成


## 思路

## 这题就是最长连续不重复子序列的字符版[最长连续不重复子序列−【双指针】（三种语言）](https://www.acwing.com/solution/content/191751/)


双指针，`i` 在后 `j` 在前，维护一段不存在重复子序列的区间。


每当 `i` 向后移的情况下， 势必 `j` 只需要向后移动，因为如果 `j` 右移代表出现重复字符，可以取左边的值，证明向左移动这个范围区间没有重复字符，显然不可能，当前都已经重复了，更别说扩大范围。

数字可以用  `int` 数组存出现次数，而字符显然用哈希表更合适。


## java

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<Character,Integer> map = new HashMap<Character,Integer>();
        int ans = 0;

        for(int i = 0,j = 0;i < s.length();i ++) {
            map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0) + 1);

            while(map.get(s.charAt(i)) > 1) {
                map.put(s.charAt(j), map.get(s.charAt(j)) - 1);
                j ++;
            }
            ans = Math.max(ans, i - j + 1);
        } 
        return ans;
    }
}
```