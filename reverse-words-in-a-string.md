[151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

给你一个字符串 `s` ，请你反转字符串中 **单词** 的顺序。

**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。

**注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

 

**示例 1：**

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**示例 2：**

```
输入：s = "  hello world  "
输出："world hello"
解释：反转后的字符串中不能存在前导空格和尾随空格。
```

**示例 3：**

```
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，反转后的字符串需要将单词间的空格减少到仅有一个。
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 包含英文大小写字母、数字和空格 `' '`
- `s` 中 **至少存在一个** 单词



这个题目的有两个点，第一个 删除多余的括号，和反转单词

首先删除多余的括号就比较简单

有几种情况考虑

1. 如果开始就是一串括号，那我们用while 来遍历，直到找到第一个不是空格的索引

   ```
   while( s[index++] ==' ' )
   ```

2. 在中间环节 如果连续两个都是空格，那么跳过

   ```
   if(s[i]==' '&&s[i+1]'  ')
    	continue;
   ```

3. 记得判断最后一个是否是空格

   ```
   if(i==n-2 && s[n-1] != ' ' )
       {
           str+=s[n-1];
       }
   ```

   然后开始反转

   反转你想到了前面的轮转数组了嘛？

   我们先找将调整空格的整个string 反转

   在根据空格分隔开单词，单词再反转一遍，那么这道题就解决了

   abc efg

   gfe cba

   efg abc

   完整代码

   ```
   class Solution {
   public:
       void doRever(string& s, int begin,int end)
       {
           while(begin < end)
           {
               swap(s[begin++],s[end--]);
           }
           
       }
       string reverseWords(string s) {
   
   
           int n = s.size();
           if(n==1)
           {
               return s;
           }
           int start = 0;
           while(s[start] ==' ')
           {
               start++;
           }
           string str;
           for(int i = start;i<n-1;i++)
           {
               if(s[i]==' ' && s[i+1] == ' ')
               {
                   continue;
               }
               str += s[i];
               if(i==n-2 && s[n-1] != ' ' )
               {
                   str+=s[n-1];
               }
           }
           
           n = str.size();
           doRever(str,0,n-1);
           start = 0;
           for(int i = 0 ;i < n;i++)
           {
               if(str[i]==' ')
               {
                   doRever(str,start,i-1);
                   start = i+1;
               }
               else if(i==n-1)
               {
                   doRever(str,start,i);
               }
           }
           return str;
   
       }
   };
   ```

## 解法二

对于这个题目？把后面的单词反转到前面 是否想到了一个数据结构 ，天生就是干这个的 先进后出，后进先出

没错 就是栈 stack

将单词挨个入栈，再出栈那么就解决了

```
class Solution {
public:
    string reverseWords(string s) {


        int n = s.size();
        if( n == 1)
        {
            return s;
        }
        stack<string> stk;
        int start = 0;
        while(s[start] ==' ')
        {
            start++;
        }
        string str;
        for(int i = start;i<n-1;i++)
        {
            if(s[i]==' ' && s[i+1] == ' ')
            {
                continue;
            }
            str += s[i];
            if(i==n-2 && s[n-1] != ' ' )
            {
                str+=s[n-1];
            }
        }
        string curWorld;;
        start = 0;
        n = str.size();
        for(int i = 0 ;i < n;i++)
        {
            if(str[i]==' ')
            {   
                curWorld = str.substr(start,i-start);
                stk.push(curWorld);
                start = i+1;
            }
            else if(i==n-1)
            {   
                curWorld = str.substr(start,i+1-start);
                stk.push(curWorld);
            }
        }
        str = "";
        while(1)
        {
            str +=stk.top();
            stk.pop();
            if(stk.empty())
            {
                break;
            }
            str +=' ';
        }
        return str;

    }
};
```



