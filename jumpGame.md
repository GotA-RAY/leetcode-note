# 跳跃游戏

[55. 跳跃游戏 - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game/description/?envType=study-plan-v2&envId=top-interview-150)

给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

**示例 2：**

```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

此题目我最先想到用递归的方法进行解答，vscode 上面编译通过但是，提交leetcode的时候说我超时

但是还是想记录一下思路

首先判要找到能不能跳到最后的一格，要想的是，我的nums[i] + i 最大能到哪里，遍历下来

如果这个组合能大于或者等于最后一个一索引 那么我们就可以跳跃到这个位置

由于这没有通过那么先写一下伪代码讲述递归过程

```
bool recJump(vector<int>&nums,int index)
{
	if(nums[index]+index >= nums.size()-1)
	{
		//递归出口
		return ture;
	}
	//不然就继续递归
	//但是这里的递归次数取决于你当前的nums[index]的值，他是你能走的步数
	bool isSuccess = false;// 记录下递归的成功状态，有一次能跳到，那就说明可以成功
	for(int i = 1;i<=nums[index];i++)
	{
		isSuccess = isSuccess || recJump(nums,index+i);
	}
	return isSuccess;
}
```

递归的代码就是这样，虽然超时了，但是还是证明了自己的思考过程，同时也说明了代码需要优化

我第一遍优化的时候，想了一下，不能说每种情况都要去递归啊，这样太浪费了，直接去当前步数下的，最大的 nums[index] + index 去递归然后判断能不能解决问题

这里有了一点贪心算法的意思了，然后事实上后面的解法也确实是用的贪心算法，只是我没有接触过不知道叫什么，该怎么做

因此我接着设计了一个函数去寻找当前步数能到达的最大的index + nums[index]的值的下标，用新的下标去完成我们递归，（但是最后还是超时。。。。。。。。。。。。。

```
int findMaxStepIndex(vector<int>& nums,int index)
    {
        int maxStep = 0;
        int maxStepIndex = 0;
        for(int i = 1;i <= nums[index];i++)
        {
            int curStep = nums[i+index]+i+index;

            if(curStep > maxStep)
            {
                maxStep = curStep;
                maxStepIndex = i+index;
            }
        }
        return maxStepIndex;
    }
```

上面的就当看个乐子吧

下面言归正传，会到正确解法，我也是看了一下题解才意识到我完全可以不用递归啊，直接找到最大的下标和下表对应的值完成跳到最后的判断，如果最大的都跳不到，那我还跳啥呢，直接return false啊

于是有了下面的

```
class Solution {
public:
  bool canJump(vector<int>&nums)
   {
    if(nums.size()<2)
    {
        return true;				//只有一个元素，那还跳什么 已经到了啊 return true
    }
    int curIndex = 0;				//为了记录当前的进去循环的基础索引，后面的索引都是基于这个索										//引开始工作的
    int curStep = nums[curIndex] ;	//当前索引的对应的值，以为这在这次循环中我们能走多少步
    //int maxStepIndex = 0;
    int maxStepCanReach = nums[curIndex] + curIndex; //当前索引记录一下我们最远能到的位置
    int i = 1;						//这个i 是为了在进去循环后，在基础的index下前进的步数，
    								//肯定是从1开始的，=当前的最大的值结束，等于不就是原地踏步										//的了吗
    while(i<=curStep)
    {

        if(maxStepCanReach>= nums.size()-1)
        {
            return true;			//到达，返回true
        }
        if(nums[curIndex+i]+curIndex+i > maxStepCanReach)	
        {//这里就是如果我们能到更远的地方，那我们就立刻更新，并重新开始我们新的一贪
            curIndex = curIndex+i;
            curStep = nums[curIndex];
            maxStepCanReach = curIndex + curStep;
            i = 1;
            continue;
        }
        i++;

    }
    return false; //什么？上面没贪到？那么我们的这个就是到不了了，最远的都到不了，那着一盘就废了
   }
};
```

## 跳跃游戏II

[45. 跳跃游戏 II - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game-ii/description/?envType=study-plan-v2&envId=top-interview-150)

给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向前跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:

- `0 <= j <= nums[i]` 
- `i + j < n`

返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。

 

**示例 1:**

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**

```
输入: nums = [2,3,0,1,4]
输出: 2
```

上一把都跳过了 接着继续跳，写下来之后，我总结 这道题你还得贪，而且要更贪啊，因为他说的要你在最少的步数到达，不就是说你要贪多一点，你在你的索引加索引对应的值能到范围内，你要找到最大的那个，而不是说比之前的大就行了。

这就是这道题的关键所在。

```
class Solution {
public:
    int jump(vector<int>& nums) {
        if(nums.size()<2)
        {
            return 0;
        }
        int ans = 0;
        // 记录答案用的
        int candidateIndex = 0;
        // 下一个最贪的候选人
        int curIndex = 0;
        // 当前最贪的
        int candidateStep = nums[candidateIndex];
        // 下一个最贪的候选人能走多少步记录一下
        int curStep = nums[curIndex];
        // 当前最贪的能走多少步
        int maxStepCanReach = curIndex + curStep;
        // 当前最贪的能走多远
        int maxCandidateStepCanReach = candidateIndex + candidateStep;
        // 候选最贪的能走多远
        int index = 1;
        while(index <= curStep)
        {
            if(maxStepCanReach >= nums.size()-1)
            {
                ans++;
                // 当前的贪g以及逃出国了啊，将出国这下要加上，跳出这个循环
                break;
            }
            if(nums[curIndex+index]+curIndex+index > maxCandidateStepCanReach)
            {
            	//候选人之间竞争，留下最贪那个作为下一任
                candidateIndex = curIndex+index;
                candidateStep = nums[candidateIndex];
                maxCandidateStepCanReach = candidateIndex + candidateStep;
            }
            if(index == curStep && maxCandidateStepCanReach>maxStepCanReach)
            {
            	//当前这人看来是走到尽头也没逃出去，被逮捕了
            	//那么我们从新任命一位在众多贪g 中选出来的最贪的以为作为鹅城新的市长
            	// 新官来了，那么我们推翻旧证，从头来过
                index = 1;
                curIndex = candidateIndex;
                curStep = candidateStep;
                maxStepCanReach = maxCandidateStepCanReach;
                ans++;
                //上一任贪g还是留下了一些作用的他到当前这一任，也是有有效的贡献的，在答案上计他一攻
                continue;
            }
            index++;
        }
        return ans;
        }
 };
```

