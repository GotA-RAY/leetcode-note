[14. 最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)

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

这道题看标签，简单，那么就不多想，遍历肯定能做出来

但是注意这是一个二维数组

要考虑第二个维度的数组越界的问题

这点注意了好说

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int n = strs.size();
        return longestCommonPrefix(strs,0,n-1);
    }
    string longestCommonPrefix(vector<string>& strs,int begin, int end)
    {
        if(begin == end)
        {
            return strs[begin];
        }
        int mid = (begin + end)/2;
        string leftlcp = longestCommonPrefix(strs,begin,mid);
        string rightlcp = longestCommonPrefix(strs,mid+1,end);
        return longestCommonPrefix(leftlcp,rightlcp);
    }
    string longestCommonPrefix(string leftlcp,string rightlcp)
    {

        int minlen = min(leftlcp.size(),rightlcp.size());
        for(int i = 0;i<minlen;i++)
        {
            if(leftlcp[i] != rightlcp[i])
            {
                return leftlcp.substr(0,i);
            }
        }
        return leftlcp.substr(0,minlen);
    }
};
```

### 解法2 分治

看了题解 才知道这题可以用分治，而且很好理解

首先我们要比较全部的最长公共前缀LCP

那么我们可以将整个要比较的内容从中间分开，先拿出前面的LCP ,再拿出后面的LCP

然后再看看这个两个LCP的LCP

上面这个思路可以继续分下去

分到最小的问题就是 相邻的两个LCP的比较

最后将结果汇总，拿到整个strs的LCP

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int n = strs.size();
        return longestCommonPrefix(strs,0,n-1);
    }
    string longestCommonPrefix(vector<string>& strs,int begin, int end)
    {
        if(begin == end)
        {
            return strs[begin];
        }
        int mid = (begin + end)/2;
        string leftlcp = longestCommonPrefix(strs,begin,mid);
        string rightlcp = longestCommonPrefix(strs,mid+1,end);
        return longestCommonPrefix(leftlcp,rightlcp);
    }
    string longestCommonPrefix(string leftlcp,string rightlcp)
    {

        int minlen = min(leftlcp.size(),rightlcp.size());
        for(int i = 0;i<minlen;i++)
        {
            if(leftlcp[i] != rightlcp[i])
            {
                return leftlcp.substr(0,i);
            }
        }
        return leftlcp.substr(0,minlen);
    }
};
```

