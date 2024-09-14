# $$\color{Red}{常数时间插入、删除和获取随机元素【LeetCode 380 哈希结合数组】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

实现`RandomizedSet` 类：

- `RandomizedSet()` 初始化 `RandomizedSet` 对象
- `bool insert(int val)` 当元素 `val` 不存在时，向集合中插入该项，并返回 `true` ；否则，返回 `false` 。
- `bool remove(int val)` 当元素 `val` 存在时，从集合中移除该项，并返回 `true` ；否则，返回 `false` 。
- `int getRandom()` 随机返回现有集合中的一项（测试用例保证调用此方法时集合中至少存在一个元素）。每个元素应该有 **相同的概率** 被返回。

你必须实现类的所有函数，并满足每个函数的 **平均** 时间复杂度为 `O(1)` 。

 

**提示：**

- `-231 <= val <= 231 - 1`
- 最多调用 `insert`、`remove` 和 `getRandom` 函数 `2 * 10^5` 次
- 在调用 `getRandom` 方法时，数据结构中 **至少存在一个** 元素。



### 思考为什么单个 Set 或者 Map 不行？

`Set` 和 `Map` 可以做到 维持单个，`O(1)的插入，删除` ，但是 `哈希值是随机分布 无法随机抽取`

这里类似 `redis` 的 `set` 想要随机选取，最好是定列值的数组，可以直接随机数选取，所以这种情况下我们需要配合一个 `list`

那么我们可以 `map`  `key 为插入的数字`，`value` 存在数组的下标，而数组位置存值

但是删除的时候，为了不出现反复移位，应该直接把当前数和数组尾部的数字交换，然后删除队尾，同时 `map` 也需要更新替换数字的新下标，然后删除要删除的数字。

```java
class RandomizedSet {

    Random random = new Random();
    HashMap<Integer, Integer> map = new HashMap<>();
    ArrayList<Integer> list = new ArrayList<>();

    public RandomizedSet() {

    }
    
    public boolean insert(int val) {
        if (map.containsKey(val)) return false;
        list.add(val);
        map.put(val, list.size() - 1);
        return true;
    }
    
    public boolean remove(int val) {
        if (!map.containsKey(val)) return false;
        int index_x = map.get(val);
        // 列表和map都需要换位置 并删除
        map.put(list.get(list.size() - 1), index_x);
        map.remove(val);
        list.set(index_x, list.get(list.size() - 1));
        list.remove(list.size() - 1);
        return true;
    }
    
    public int getRandom() {
        int id = random.nextInt(list.size());
        return list.get(id);
    }
}
```

