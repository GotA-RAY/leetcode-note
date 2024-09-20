## [58. 最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)



相关标签

相关企业



给你一个字符串 `s`，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 **最后一个** 单词的长度。

**单词** 是指仅由字母组成、不包含任何空格字符的最大

子字符串

。



 

**示例 1：**

```
输入：s = "Hello World"
输出：5
解释：最后一个单词是“World”，长度为 5。
```

**示例 2：**

```
输入：s = "   fly me   to   the moon  "
输出：4
解释：最后一个单词是“moon”，长度为 4。
```

**示例 3：**

```
输入：s = "luffy is still joyboy"
输出：6
解释：最后一个单词是长度为 6 的“joyboy”。
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅有英文字母和空格 `' '` 组成
- `s` 中至少存在一个单词

这道题也是比较简单的，我们直接倒叙遍历找到第一个单词的第一个字母firstAlpha和最后一个字母lastAlpha的位置然后 两个相减加一就好了

但是关于这两个位置的判断的一些逻辑需要整理清楚

第一个

如果最后一个索引s[n-1] 不是空格的话记录下来为firstalpha，或者s[i-1]不是空格，s[i]是空格,那我们记录我们的i-1为firstalpha

最后一个

这个需要处理的就是当前的这个索引是否为s的起始索引也就是0 s[0]不是空格，那么就记录0为lastalpha

或者s[i]不是空格s[i-1]是空格，那么我们记录i为lastalpha

```
class Solution {
public:
    int lengthOfLastWord(string s) {
        int n = s.length();
        int firstAlpha = -1;
        int lastAlpha = -1;
        for(int i = n-1;i>-1;i--)
        {
            if(i==n-1 && s[i]!=' ')
            {
                firstAlpha = i;
            }
            else if( s[i]==' ' && s[i-1]!=' ')
            {
                firstAlpha = i-1;
            }
            if(i==0&&s[i]!=' ')
            {
                lastAlpha = i;
                break;
            } 
            else if(s[i] != ' ' && s[i-1] == ' ')
            {
                lastAlpha = i;
                break;
            }

            
        }
        return firstAlpha - lastAlpha+1;
    }
};
```

