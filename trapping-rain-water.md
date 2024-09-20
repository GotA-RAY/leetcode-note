[42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)



给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例 2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

 

**提示：**

- `n == height.length`
- `1 <= n <= 2 * 104`
- `0 <= height[i] <= 105`



这道题 似乎是很经典，以至于我在抖音都刷过两边，但是还是第一次写

这道题，使用栈来解决，为什么用栈呢

首先栈的特性是先进后出，每次出的都是栈顶的元素

对于这题，怎么接雨水呢，是不是中间有个凹槽就能接了，换句话说凹槽，就是左边小于右边，

但是左边小于右边是出现凹槽的充要条件嘛？

有槽，能推出中间小于两边，从而能推出，左边小于右边的时候，可能有槽出现，因为我们的左边是否大于中间呢？没说啊

那么我们就来假设这左边是大于中间的，最后如果不是，我们假设接到的水倒出来就好了，

我们只能判断其中一格，因此接到的话就要将原来的那个凹槽用水填满，防止下次再接一次

例如

[0,1,0,2,1,0,1,3,2,1,2,1]

前两个元素 0 < 1,现在出现了左边小于右边的情况我们先接起来，然后再判断

怎么0的左边没有元素啊，我们将水吐出

对于为什么用栈呢，

首先我们栈如果空的话，我们先将当前元素入栈，然后进到下一个

如果下个小于右边，那么继续入栈，如果大于右边，

进入一个循环，循环内

将现在的栈顶取出来

作为接水的水坑最低值，然后假设他左边界跟右边界一样，然后计算这个能接多少水，然后，将这个坑填满

填到多少是满呢，那自然是跟边界值一样了

再继续判断是否还是左边小于右边的值，如果不是的话就跳出这个循环

对原来的数组按上面逻辑入栈

```
class Solution {
public:
    int trap(vector<int>& height) {
        stack<int>st;
        int n = height.size();
        st.push(height[0]);
        int weight = 0;
        int ans = 0;
        int last = 0;
        for(int i = 1;i<n;i++)
        {
            last = st.top();
            if(height[i] <= last)
            {
                st.push(height[i]); 
            }
            else
            {
                while(height[i] > last)
                {
                    
                    st.pop();
                    weight++;
                    ans+=height[i] - last;    
                    if(st.empty()) // 这里就要判断要不要吐出来了
                    {
                    	//逻辑就是，如果取出来的最后一个值还是小于当前的height[i]，就说明
                    	// 当前栈里的所有值都是小于右边界的，那么就要吐出来的刚刚接的水
                    	
                        ans -= (height[i] - last)*weight;
                        weight = 0;
                        break;
                    }
                    last = st.top();

                    
                }
                for( int j = 0;j < weight+1;j++)
                {
                    st.push(height[i]);
                    // 这里就是接完了填坑种
                    
                }
                weight = 0;
           
            
            }
        }
        return ans;
    }
};

```





还有一种解法也是用栈，不过不是傻乎乎的记录值，然后去填坑，而是直接记录水坑边界的位置

```
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        int n = height.size();
        stack<int>st;
        int left = 0;
        int mid = 0;
        int width = 0;
        int high = 0;
        for(int i = 0; i < n;i++)
        {
            while (!st.empty() && height[i] > height[st.top()])
            {
                mid = st.top();
                st.pop();
                if(st.empty())
                {
                    break;
                }
                left = st.top();
                high = min(height[i],height[left]);
                high = high - height[mid];
                width = i-left-1;
                ans+=width*high;
            }     
           st.push(i);            
        }
        return ans;
    }   
    
};
```

他的逻辑是如果height左边的大于右边，先入栈当前的索引，就是水坑边界的位置

然后如果遇到height左边小于右边，那么此时前面就有坑了，

取出栈顶的值作为坑的底，弹出当前，再去取出栈顶，这个元素对应的height数组的值作为这个水坑的左边界，然后比较左边界和右边界height[i]取小的，作为这个水坑的有效高度，然后用有效高度，减去底高，作为实际深度，然后用i-left-1，算出这个水坑有多宽，用宽度乘以深度就是我们当前的这个水坑的储水量；