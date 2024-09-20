# 日常刷题之双指针

## 删除有序数组

题目来源leetcode 面试经典150题

[26. 删除有序数组中的重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150)

给你一个 **非严格递增排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

读题：非严格递增数列，即nums[i] <= nums[i+1],

原地删除，说明不能创建新的容器，申请新的内存来完成

思路：第一眼看见这个题目，想到用快慢指针来完成，通过比较快指针和慢指针的指来完成对数组的遍历的同时将出现过多次的值替换掉

解法一，自己的快慢指针思路

比较快指针跟慢指针的值，如果相等则将快指针加一，当快指针跟慢指针不相等的时候，将慢指针加一，然后新的慢指针等于快指针的值。

但是最后为什么是返回s+1呢？

看下面这个数组

1122334   f = 0 s= 0

按照这个算法执行时，依次会有

1222334 f = 2 s =1

1232334 f=4  s=2

1234334 f=6 s=3

进入最后一次循环时 nums[f] = 4 ,nums[s] =4 ,f++,跳出循环

但是此时s=3 而非4，答案要的是输出数组的size 而不是下标索引；

这是因为，这样的弊端，到最后一个数字的时，



```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int f = 0;
        int s = 0;
        while( f < nums.size())
        {
            if(nums[s] == nums[f])
            {
                f++;
            }
            else
            {
                s++;
                nums[s] = nums[f];
            }
        }      
        return s+1;
    }
};
```

但是后面看了一下解析，发现可以将 nums[s-1] 和 nums[f]比较，这样如果 两个不一样，说明，可以将nums[s]的值赋成nums[f]的值，而唯一保证nums[s-1]的值是唯一的，然后此时将s的索引加一，这样就可以直接返回s而不是s+1，此时维护的是s-1独一无二，则第 0 ~ s-1 个数不重复，此时就返回前s 个数就行

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if(n <2)
        {
            return n; //如果nums的大小小于 2 则 每个数都是独一无二的
        }        
        int s = 1, f = 1;
        while(f < n)
        {
            if(nums[s-1]!=nums[f])
            {
                nums[s] = nums[f];
                ++s;
            }
            ++f;
        }
        return s;
    }
};
```

## 删除有序数组中重复项II

[80. 删除有序数组中的重复项 II - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/?envType=study-plan-v2&envId=top-interview-150)

给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

此题跟上一题思路一致，使用快慢指针，但是区别在于上一个题目要求保留一个数，改题目可以最多保留两个一样的数

解题思路：

最开始，想到由于是最多出现两次，那么大多数人反应就是在声明完快慢指针后，再搞一个计数器，来记录出现的次数，没错，我最开始也是这样想的，但是马上反应过来了，为什么要多次一举呢？

这出现的次数完全可以用f-s来记录啊，而且这样维护起来也方便，

f走在前面，发现了第一个跟s不一样的数的时候停下来，那么这会更上面那道题目一样，有两种情况了

f-s <= 2,则满足要求，可以将s更新到f的位置，开始新的数字的遍历

如果f-s > 2了那么就是说明 此时不满足要求了，就要开始对数组进行操作了，

我开始想到了这，思路跟leetcode 题解思路类似，但是后面就开始犯傻了

下面是我的犯傻记录

将s更新到s+2，然后将从f开始的数字全部向前移动 f-s个位置，然后此时nums的有效长度也跟着减小f-s个

例如

11122223

第一次遍历的时，f=3，s = 0，此时f-s = 3 不满足要求 要进行调整，s+=2 

那么此时 nums[f] 到 nums.size()-1 的所有数字要往前移动 f-s 3-2 =1 个下标，此时 nums的有效长度也会跟着减小1

然后将f 会到s 当前的值，继续往复，直到跳出循环

跳出循环后，依然不能直接返回s，要对s进行判断，

因为维护的是当前s的值，那么整个返回数组的长度肯定大于s，要是最后一个数只出现了一次，那么就是返回s+1，（0~s之间有s+1个数），要是最后一个数字出现了两次，那么就要返回s+2了，

怎么判断呢，很简单，如果s = nums更新后的有效长度-1，那么s就是最后一个数字了，最后一个只出现了一次，反之出现了两次。

**很蠢！！！！** 不过先提交了再说

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        
        if(nums.size()<=2)
        {
            return nums.size();
        }
        int s = 0;
        int f = 0;
        int cnt = nums.size();
        while(f < cnt)
        {
            if(nums[s] == nums[f] )
            {
                f++;
                
            }
            else
            {
                if(f-s<=2)
                {
                    s = f;
                }
                else
                {
                    s = s+2;
                    int diff = f - s;
                    for(int i = f;i<cnt;i++)
                    {
                        nums[i-diff] = nums[i];
                    }
                    cnt -= diff;
                    f = s;
                }
            }
        }
        if(s == cnt -1)
        {
            return s+1;
        }
        if(nums[s] == nums[s+1])
        {
             s+=2;
            
        }
        return s;
       
    }
};
```



后面看到题解，顿悟，可以通过维护s-2 跟 f的关系来完成此题，那么就跟上一道题一样了

如果s-2 跟 f 不一样那么就跟新s 为新的f的值就好了 因为这样可以保证 至多有 s-1 s-2 两个值一致、

(为什么最多保证s-1 s-2一致

解答：如果s-2 跟 f一样，f>=s,

那么就有 s-2 s-1 s，三个值一样，此时我们就要f继续遍历然后更新s为f出现的第一个不一样的值

那么此时最多就只能维护s-2 s-1 一样

那如果 s-2 跟s-1不一样，

有f > = s 那么s跟s-2 一定不一样

11123变为11223，此时s=3 ，f = 4 nums[s] = 2, nums[s-2] = 1 nums[f] = 3,此时这个，

f跟s-2的值一定不一样，那么这个新的值一定是满足要求的，所以可以更新到当前s的位置

只有当s-2，跟f相等的时候，此时这个f当前值不能要，要继续向前遍历得到新的值来更新我们的s指向的值

我们的核心是s的值一定是新的

)

然后将s++

例如

11112222333

第一次s=2 f=2，

那么此时s-2 明显跟f一致，往后遍历，

f = 4 的时候不一样

那么将s 跟新为2

11212222333，改完后f 也要往后移

s = 3，f = 5

不一样更新

11222222333

s=4，f=6

一样，不更新，f向后走

直到f=8，

更新

11223222333

s=5 f=8继续更新

11223322333

s=6，f =9

往后f=10的时候还是3，那么就不更新，然后跳出循环结束

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if(n<=2)
        {
            return n;
        }
        int f=2,s=2;
        while(f<n)
        {
            if(nums[f] == nums[s-2])
            {
                f++; //此时就是上面说的不能要场景，f继续向前
            }
            else
            {
                nums[s++] = nums[f++]; //其他情况都是要将f的值留下成当前的s，更新完nums[s]的值，记得继续将f s都继续向下一个数比较
            }
        }
        return s;
    }
};
```







