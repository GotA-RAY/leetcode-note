[6. Z 字形变换](https://leetcode.cn/problems/zigzag-conversion/)

将一个给定字符串 `s` 根据给定的行数 `numRows` ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"PAYPALISHIRING"` 行数为 `3` 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"PAHNAPLSIIGYIR"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

 

**示例 1：**

```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```

**示例 2：**

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

**示例 3：**

```
输入：s = "A", numRows = 1
输出："A"
```

 

**提示：**

- `1 <= s.length <= 1000`
- `s` 由英文字母（小写和大写）、`','` 和 `'.'` 组成
- `1 <= numRows <= 1000`

这道题怎么解决呢，最简单的就是模拟这个情况，一行行输入

到最下面的哪一行之后，再一行行的往上走，到最上面的一行后，再一行行的往下走

其中往下还是往上使用一个flag 来区分

```
class Solution {
public:
    string convert(string s, int numRows) {
        
        int n = s.size();
        if(numRows == 1)
        {
            return s;
        }
        vector<string> strs(numRows);
        int curRow = -1; // 这里特殊处理，后面的才能完成正确的遍历
        bool flag = false;
        for(int i = 0;i<n;i++)
        {
            if(!flag)
            {
                ++curRow;
                strs[curRow] += s[i];
                if(curRow==numRows-1)
                {
                    flag = true;
                }
            }
            else
            {
                --curRow;
                strs[curRow] +=s[i];
                if(curRow==0)
                {
                    flag = false;
                }
            }
        }
        string res = "";
        for(auto str : strs)
        {
            res+=str;
        }
        return res;
    }
};
```

