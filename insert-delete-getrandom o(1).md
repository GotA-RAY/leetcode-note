# insert-delete-getrandom o(1)

https://leetcode.cn/problems/insert-delete-getrandom-o1/?envType=study-plan-v2&envId=top-interview-150

实现`RandomizedSet` 类：

- `RandomizedSet()` 初始化 `RandomizedSet` 对象
- `bool insert(int val)` 当元素 `val` 不存在时，向集合中插入该项，并返回 `true` ；否则，返回 `false` 。
- `bool remove(int val)` 当元素 `val` 存在时，从集合中移除该项，并返回 `true` ；否则，返回 `false` 。
- `int getRandom()` 随机返回现有集合中的一项（测试用例保证调用此方法时集合中至少存在一个元素）。每个元素应该有 **相同的概率** 被返回。

你必须实现类的所有函数，并满足每个函数的 **平均** 时间复杂度为 `O(1)` 。

 

**示例：**

```
输入
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
输出
[null, true, false, true, 2, true, false, 2]

解释
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomizedSet.remove(2); // 返回 false ，表示集合中不存在 2 。
randomizedSet.insert(2); // 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomizedSet.getRandom(); // getRandom 应随机返回 1 或 2 。
randomizedSet.remove(1); // 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomizedSet.insert(2); // 2 已在集合中，所以返回 false 。
randomizedSet.getRandom(); // 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
```





这道题要我们设计一个类，要求是O(1)的时间复杂度插入，查询，以及删除。

要解决这题，得先想想用什么数据结构取存放我们的数据，

我最开始想到用类似list的方法实现，我们的直接删除复杂度为O1,但是这是一个set，元素不能重复，因此在每次操作时我们要先遍历一遍，这样的问题就大了啊，所以想到这，想用一个unordered_map记录一下，利用它来记录数字出现次数，是不是以为一个unordered_map就够了

我也是这样想的，可能是我笨，随机返回我想不到怎么用一个unordered_map解决，所以还是得配合一个vector来完成随机返回，通过srand 产生索引，来返回， unordered_map,来完成记录是否存在该值



但是！ 有个问题 ，vector的删除的时间复杂度是？

我记得我当时学的时候，说如果，要删除的元素在末尾，那么我们的删除这个元素只用简单的pop_back,此时我们时间复杂度为O1,BUT，如果不是的话就难了，vector是**一个动态的连续的**的数据结构，如果要删除一个在中的话，意味着我们要把后面每一个移到前面来，那么此时的时间复杂度就是O(n)了

为了验证我的记忆 附上kimi ai的解释（gpt的ip给ban了 而且kimi挺好用的）

在C++中，`std::vector`是一个连续存储的动态数组。删除`vector`中的元素的时间复杂度取决于你要删除的元素的位置以及`vector`的大小。



### 删除特定位置的元素

如果你要删除的是`vector`中特定位置的元素，那么时间复杂度如下：

- **单个元素**：如果删除的是除了末尾之外的任意位置的元素，那么时间复杂度是 𝑂(𝑛)*O*(*n*)，其中 𝑛*n* 是从删除位置到末尾的元素数量。这是因为你需要将该位置之后的所有元素向前移动一位，以填补被删除元素的位置。
- **末尾元素**：如果删除的是`vector`的最后一个元素，时间复杂度是 𝑂(1)*O*(1)，因为只需要简单地减少`vector`的大小，不需要移动任何元素。

### 删除特定值的元素

如果你要删除`vector`中所有匹配某个特定值的元素，那么时间复杂度通常也是 𝑂(𝑛)*O*(*n*)，其中 𝑛*n* 是`vector`中的元素总数。这是因为你需要遍历整个`vector`来查找并删除匹配的元素。每次删除操作都需要移动元素，所以整个操作的时间复杂度是线性的。

### 注意事项

- 删除操作可能涉及内存的重新分配，如果`vector`需要扩展其当前分配的内存空间以容纳更多的元素。这可能会增加删除操作的时间，因为内存分配通常是一个较慢的操作。
- 如果`vector`中的元素是动态分配的或者包含有资源（如内存、文件句柄等），则还需要考虑释放这些资源的时间。
- 如果`vector`中的元素具有复杂的构造函数或析构函数，那么删除操作的时间复杂度也可能受到构造和析构操作的影响。
- 在某些情况下，为了提高删除操作的性能，可以考虑使用`std::list`或`std::deque`，这些容器提供了更快的插入和删除操作，尤其是在列表中间位置。
- 如果你经常需要从`vector`中删除元素，而且对随机访问有较高要求，可以考虑使用`std::vector`的特殊版本，如`std::vector<T>::iterator`或`std::vector<T>::reverse_iterator`，它们提供了一种更高效的方式来删除元素，因为它们允许你直接在迭代器上操作，而不需要遍历整个容器。

总的来说，`std::vector`的删除操作在最坏情况下的时间复杂度是 𝑂(𝑛)*O*(*n*)，但在某些情况下，如删除末尾元素，可以是 𝑂(1)*O*(1)。





所以直接删除，是不可能的，好在 文中提到了要是 要是  **要删除的元素在末尾，那么此时的时间复杂度为O(1)** 柳暗花明了

所以我们将我们要删除的元素跟末尾的换个位置，然后删掉就好了，有没有要求顺寻，孙旭 顺寻 顺序，对吧

换的时候记得把unordered_map的对应的索引也一同换下。

开始写

```
class RandomizedSet {

    vector<int>_data;
    unordered_map<int,int>_indicate;

public:
    RandomizedSet() {
        srand(time(0));
    }
    
    bool insert(int val) {
        if(_indicate.count(val)!=0)
        {
            return false;
        }
        int index = _data.size();
        _indicate[val] = index;
        _data.emplace_back(val);
        return true;
    }
    
    bool remove(int val) {
        if(_indicate.count(val) == 0)
        {
            return false;
        }
        int last = _data.back();
        int index = _indicate[val];
        _data[index] = last;
        _indicate[last] = index;
        _data.pop_back();
        _indicate.erase(val);
        return true;
    }
    
    int getRandom() {
        int index = rand()%_data.size();
        return _data[index];

    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

