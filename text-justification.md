[68. 文本左右对齐](https://leetcode.cn/problems/text-justification/)

给定一个单词数组 `words` 和一个长度 `maxWidth` ，重新排版单词，使其成为每行恰好有 `maxWidth` 个字符，且左右两端对齐的文本。

你应该使用 “**贪心算法**” 来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格 `' '` 填充，使得每行恰好有 *maxWidth* 个字符。

要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。

文本的最后一行应为左对齐，且单词之间不插入**额外的**空格。

**注意:**

- 单词是指由非空格字符组成的字符序列。
- 每个单词的长度大于 0，小于等于 *maxWidth*。
- 输入单词数组 `words` 至少包含一个单词。

 

**示例 1:**

```
输入: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
输出:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

**示例 2:**

```
输入:words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
输出:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
解释: 注意最后一行的格式应为 "shall be    " 而不是 "shall     be",
     因为最后一行应为左对齐，而不是左右两端对齐。       
     第二行同样为左对齐，这是因为这行只包含一个单词。
```

**示例 3:**

```
输入:words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"]，maxWidth = 20
输出:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

 

**提示:**

- `1 <= words.length <= 300`
- `1 <= words[i].length <= 20`
- `words[i]` 由小写英文字母和符号组成
- `1 <= maxWidth <= 100`
- `words[i].length <= maxWidth`

这道题利用到贪心算法，局部最优换取全局最优

首先我们先将每行的情况处理好

我们要在固定宽度内，放置更多的单词，我们需要统计我们当前行的单词，加上最少的空格数（单词数量减一就是我们所需要的最少的空格数）

如果大于了我们最大的限制，那么此时最后一个单词肯定是不能要的，然后将这个单词前面选中的单词通过计算，算出需要在哪里补多少个空格就解决了

最后一行需要单独处理，因为最后一行肯定会小于最大的限制，如果还按照上面的逻辑进行判断，那么肯定是不行的，最后一行不全空格逻辑也不一样也需要特定的处理

```
class Solution {
public:
    string fixSpace(vector<string>& words,int begin,int num,int spaceNum)
    {
       string res = "";
       if(num == 1)				//如果只有个单词在当前行，那么需要在后面补全空格到最大行
       {
            res+=words[begin+num-1];
            for(int j=0;j<spaceNum;j++)
           {
               res+=' ';
           }
           return res;

       }
      
        int spaceNum1 = spaceNum/(num-1);	//这里的是两个单词之间单词数量	
        int spaceNum2 = spaceNum%(num-1);	//这里是左边多的空格数量，在每个单词之间添加
       //例如 如果除开空格 单词大小是12，有三个单词，那么需要在三个单词之间填满四个空格
       //spaceNum1 = 2；单词之间的空格数量为2
       //spaceNum2 = 0； 空格数量刚好能平均分完那么此时就不用了
       //如果有四个单词，
       //那么就没法平均分了，第一个单词间隔处要多加一个了
        
       for(int i = 0;i<num-1;i++)
       {
           res+=words[begin+i];
           for(int j=0;j<spaceNum1;j++)
           {
               res+=' ';
           }
           if(spaceNum2-- > 0)
           {
               res+=' ';
           }
       }
       res+=words[begin+num-1];
       return res;
    }
    vector<string>fullJustify(vector<string>& words,int maxWidth)
    {
        int curWidth = 0;
        int n = words.size();
        int index = 0;
        int start = 0;
        vector<string>res;
        int curWordNum = 0;
        int spaceNeed = 0;
        while(index < n)
        {
           curWidth += words[index].size();
            curWordNum = index - start+1;
            if((curWidth + curWordNum-1) > maxWidth )
            {
                curWidth -= words[index].size();
                curWordNum --;
                spaceNeed = maxWidth - curWidth;
                res.emplace_back(fixSpace(words,start,curWordNum,spaceNeed));
                curWidth = 0;
                start = index;
                continue;
            }
            else if(curWidth <= maxWidth && index == n-1
            //最后一行的处理逻辑按照题目意思就好了
            {
                string lastRow ="";
                for(int j = 0;j<curWordNum-1;j++)
                {
                    
                    lastRow+=words[j+start];
                    lastRow+=' ';
                }
                lastRow+=words[start+=curWordNum-1];
                int size = lastRow.size();
                fill_n(back_inserter(lastRow),maxWidth-size,' ');
                res.emplace_back(lastRow);
                cout << "last row size " << lastRow.size();
                break;
                
            }
            index++;
        }
        return res;

    }
};
```

