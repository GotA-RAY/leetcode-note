# h_index

 https://leetcode.cn/problems/h-index/description/?envType=study-plan-v2&envId=top-interview-150

给你一个整数数组 `citations` ，其中 `citations[i]` 表示研究者的第 `i` 篇论文被引用的次数。计算并返回该研究者的 **`h` 指数**。

根据维基百科上 [h 指数的定义](https://baike.baidu.com/item/h-index/3991452?fr=aladdin)：`h` 代表“高引用次数” ，一名科研人员的 `h` **指数** 是指他（她）至少发表了 `h` 篇论文，并且 **至少** 有 `h` 篇论文被引用次数大于等于 `h` 。如果 `h` 有多种可能的值，**`h` 指数** 是其中最大的那个。

 

**示例 1：**

```
输入：citations = [3,0,6,1,5]
输出：3 
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。
```

**示例 2：**

```
输入：citations = [1,3,1]
输出：1
```

 

**提示：**

- `n == citations.length`
- `1 <= n <= 5000`
- `0 <= citations[i] <= 1000`

这道题我觉得解法是不难的，难在怎么去理解这个h指数

题目的描述比较绕，抓住关键点，发布了至少有h篇论文，其中至少有h篇论文的引用次数大于等于h

一句话三个h，又是至少有，又是大于的，非常绕，但是读懂了这句话这道题就很简单了

假设他一共发布了n篇论文，那我们的h能不能去到n呢？

根据那句话，如果每篇的引用次数都大于n的话，那是不是h就可以取到n了

那如果第一篇小于n 后面的都大于等于n-1，就意味着我们h=n-1，还是有点难理解，

直接看例子[0,1,3,5,6]

要我们取到最大的h，那我们先可以取最大的论文数，一共五篇，然后我们再查一下最小的引用次数，最小的都次数都大于等于5，那我们h指数是不是都大于5了

所以我们可以先来个排序，把引用次数按大小排列，然后先从最大的h开始取，我们用最大的h跟最小的引用次数minCitation比较，如果满足 h>=minCitation,那我们就返回这个答案，如果不满足，就说明我们这一篇论不能纳入h指数的判断了，相应的我们的h-1，继续去比较下一个最小的，直到遍历完整个数组

```
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int h = citations.size();
        int n = citations.size();
        int minCitationIndex = 0;
        sort(citations.begin(),citations.end());
        while(minCitationIndex < n)
        {
            if(citations[minCitationIndex]>=h)
            {        
                break;
            }
            h--;
            minCitationIndex++;
        }
        return h;
    }
};
```

提交看了一下，由于使用了sort排序，导致时间复杂度到了O(nlogn),空间复杂度也到了O(logn)

于是果断吸收题解的第一个解法，计数排序，不用到sort

主要思路的就是再构造一个数组，存放引用次数的计数，下标代表引用次数，例如 citation[i] = 3,那么就将counter[3] ++; 但是我们的h最大的为n所以不小于n的，我的统统给他加到index = n的上面即 counter[n]

然后再重新遍历一遍了个这数组，倒序遍历（我们要取最大的h），将结果累加，一旦结果大于 我们遍历的数量我们就输出当前的h值

```
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size();
        vector<int>counter(n+1);
        for(int i  = 0;i < n;i++)
        {
            if(citations[i] >= n)
            {
                counter[n]++;
            }
            else
            {
                counter[citations[i]]++;
            }
        }
        int tot = 0;
        for(int i =n; i>=0;i--)
        {
            tot += counter[i]; // 这里的countor[i]为大于等于i的篇数，
            // tot为 大于等于 i~n的文章数量的总和
            if(tot >= i )
            {
                return i; // i 为 最大的h值，
            }

        }
        return 0;
    }
};
```

此解法的时间复杂度为 O(n)来自于遍历长度为n的数组，空间复杂度为O(n) 创建了一个n+1的数组放我们引用次数大于等于当前索引的文章数量，当前索引为引用数量。

### 第三种解法 尝试解读

在二分查找的边界上有点迷糊

第三种解法 采用的二分查找，核心就是在通过citations的下标 进行二分查找，如果这个 引用次数大于**等于**此时的mid 则说明 这个h的值肯定在右边[mid~n],反之就在左边了[0,mid-1]

完成查找后返回left 值 或者right值

```
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size();
        int right = citations.size();
        int left = 0;
        int cnt = 0;
        int mid = 0;
        while(left<right)
        {
            mid = (left + right +1) >> 1;
            cnt = 0;
            for(int i = 0;i<n;i++)
            {
                if(citations[i] >= mid)
                {
                    ++cnt;
                }
            }
            if(cnt >= mid)
            {
                left = mid;
            }
            else 
            {
                right = mid -1;
            }
            
            
        }
        return right;
    }
};
```

例如[3,1,0,5,6]

left = 0 ; right = 5; mid = 3;

遍历第一遍发现有3篇符合当前h值（3），说明我们的h还有可能更大，所以将左边界移动到mid，

left = 3；mid = 4；

遍历第二遍 发现大于的4的只有两篇说明此时我们的h取大了，将我们右边界移动到mid-1，此时right=3

退出循环了，然后返回答案。