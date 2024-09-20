# productOfArrayExceptSelf

https://leetcode.cn/problems/product-of-array-except-self/?envType=study-plan-v2&envId=top-interview-150

[238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

给你一个整数数组 `nums`，返回 *数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积* 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请 **不要使用除法，**且在 `O(*n*)` 时间复杂度内完成此题。

 

**示例 1:**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

**示例 2:**

```
输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

 

**提示：**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内

 

**进阶：**你可以在 `O(1)` 的额外空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组 **不被视为** 额外空间。）

这道题说要用O（1）的额外空间复杂度解决，说明除了返回的答案外，尽量不要再开辟其他空间了

首先想到这道题，能不能用暴力直接一股脑怼过去，O(N^2)的时间复杂度，提交了出现了超时

因此这个方法不妥

原因是在for循环内，还要再开一个for循环进行遍历。。

但是如果将这两个for循环不要嵌套使用，有办法吗？

想了一下还真有，题目是除了自己外的所有的元素的乘积，那么我们能不能先遍历一遍整个数组，将所有的乘积乘起来，然后再将每个答案除以自己的原来的数，不就好了

1，2，3，4 

乘积为24，

那么对应除以各自的元素

输出 24，12，8，6，很完美

但是个问题，万一出现了 0，怎么办，不能除以0，且如果再累乘的时候，会将所有的结果吞掉，到最后输出的都变成0了

对于0，很简单，我们观察例子2

-1，1，0，-3，3

输出的为 0 0 9 0 0

很明显，除了0 之外的所有元素，在输出的时候，因为0的存在，让他们除了自己的以外的所有其他元素的累乘都为0，而0 正好是除了0它本身之外的所有的元素的乘积

那么我们可以在第一次累乘遍历的时候，跳过0，将0出现的索引记录下来，然后将答案的对应的索引附上跳过0后的total

这是出现了 一个0 的情况，那要是出现了不止一个0呢，

这种情况最简单了，这样0 除了它本身之外，累乘其他的时候，也会带上其他的0，那么的他的值也是0了，结合上面的总结，这样我们答案就全是0了

```
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        
        int zeroCnt = 0;
        int zeroIndex = -1;
        int total = 1;
        int n =nums.size();
        vector<int>ans(n,0);
        for(int i =0;i<n;i++)
        {
            if(nums[i]!=0)
            {
                total*=nums[i];
                continue;
            }
            zeroCnt++;
            zeroIndex = i;
        }
        if(zeroCnt==0)
        {
            for(int i = 0;i<n;i++)
            {
                ans[i] = total/nums[i];
            }

        }
        else if( zeroCnt == 1)
        {
            ans[zeroIndex] = total;
        }
        return ans;
    }
};
```

