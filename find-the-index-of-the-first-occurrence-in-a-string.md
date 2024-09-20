[28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

 

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

**示例 2：**

```
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

 

**提示：**

- `1 <= haystack.length, needle.length <= 104`
- `haystack` 和 `needle` 仅由小写英文字符组成

这道题的话 因为是比较两个string ，所以理所应到想到双指针

如果needle的指针等于needle的size的话，那么意味着我们的字符串匹配上了，那么我们返回现在的haystack头部的指针

遍历完没返回以为这我们匹配失败，返回-1

这里的haystack的我们只用维护头部的指针就好了，遍历的时候我们通过头部加needle的指针来完成匹配

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        int haystackSize = haystack.size();
        int haystackIndex = 0;
        int needleSize = needle.size();
        int needleIndex = 0;
        while(haystackIndex < haystackSize)
        {
            while(haystack[haystackIndex+needleIndex] == needle[needleIndex])
            {
                needleIndex++;
                if(needleIndex == needleSize)
                {
                    return haystackIndex;
                }
            }
            needleIndex = 0;
            haystackIndex++;
        
        }
        return -1;
    }
};
```

