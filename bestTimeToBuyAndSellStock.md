## 买股票的最佳时机 

https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/?envType=study-plan-v2&envId=top-interview-150

最开始拿到题，先想到暴力破解，就是算出每天买入，然后在后面的日期卖出，在这些的卖出中选取最大值提交，那么这样的时间开销来到了O(N^2),空间也O(N)

然后又想到，能不能用一个双端队列deque去记录，front记录最小值，back记录最大值，如果便利的prices的值小于头部，那么更新front，大于尾部那么更新back，但是这样的思路没有考虑到时间问题，最小值和最大值的相对问题，局部最小值只能出现在最大值之前

例如出现了 21201

会将0置为front，而不是1，

后面换了一种思路，利润最大，可以简化为，找到最小值后面的最大值做差，然后如果更新了最小值，继续往后找最大值，记录新的差，看是否是最大就行了

遍历prices，更新最小值，并且如果prices[i]的值大于minPrices时考虑更新maxProfit。

例如  5 2 3 6 1 7，

更新minPrice 从2 到1 之前有，maxProfit = 6 - 2 =4；

更新完之后 maxProfit = 7 -1 = 6; 假如不更新，那么2 > 1 买入的价格大，那么利润低，这样解是对的

继续有 7 2 5 7 1 5；

此时最大差为 7 和 1但是注意 7 在1前面， 更新完最小的价格后 只能往后找 卖出的价格 此时为5 ，那么这一轮的价格就是 5-1=4，小于之前的卖出7-2=5.

假设最后的5 更大呢，等于9 10 这些甚至更大，那么此时的利润肯定不是5了而是 最后一个数减去 当前的最低价，因此我们要记录下来最大利润

于是我们的代码就是

```
int maxProfit(vector<int>& prices) 
   {
        int minPrices = prices[0];
        int ans = 0;
        int n = prices.size();

        for(int i = 1;i<n;i++)
        {
            if(prices[i] < minPrices)
            {
                minPrices = prices[i];						//当前价格小于最低价那么就要更新最低价了
            }
            else
            {
                ans = max(ans,prices[i]-minPrices);			//如果当前价格大于最低价，那么就要考虑是否要更新最大利润了
            }
        }
        return ans;
   }
```





## 买股票的最佳时机II

https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/?envType=study-plan-v2&envId=top-interview-150

给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回 *你能获得的 **最大** 利润* 。

7 1 5 3 6 4，那么最大的利润就为 5 - 1+6-3 = 7；

这道题由上一道衍生出来

一样的还是求最大利润只不过是累加，思路还是那些，只不过有点不同，需要一个记录当前这轮的最大利润 还要考虑何时更新minPrices，何时去累加，何时更新当前最大利润，我们的当前利润取最大值的时候就可以将当前利润累加进最终答案

解决这个问题 得想明白，当如果有盈利了，盈利减小的那一天的前一天一定是卖出股票的那一天

拿上面的7 1 5 3 6 4解释

当到了第三天的盈利为4 第四天的时候盈利为2，盈利减少了，那么第三天一定要卖出，后面要不要买入再重新计算，

为什么，

因为第四天之后如果有 一天价格为 x

可以讨论 x的取值，如果x>5;

我们盈利为 5-1+x-3 = x+1; 大于 6

如果  3<x<=5;

那么盈利 还是 5-1+x-3 ；大于4 

如果 x<=3

那么盈利为 5-1 = 4；

这是在第三天卖出情况，那我们来看看 第三天不卖出

1. x>5
   1.  profit = x- 1  肯定小于第三天卖出的该类情况的 x+1，
2. x<=5
   1. profit = 5-1 =4 <= 上述卖出的情况

因此我们必须有 当如果有盈利了，盈利减小的那一天的前一天一定是卖出股票的那一天

所以我们的代码

```
int maxProfit(vector<int>& prices) {
        int minPrices = prices[0];
        int ans = 0;
        int oncePrices = 0;
        int lastMostPrice = 0;
        int n = prices.size();

        for(int i = 1;i<n;i++)
        {
            if(prices[i] < minPrices)
            {
                minPrices =  prices[i];			// 一旦更新了我们minPrices 我们就必须要将答案累加
                ans+=lastMostPrice;				
                lastMostPrice = 0;
            }
            else
            {
                oncePrices = prices[i] - minPrices;
                if(oncePrices >= lastMostPrice)
                {
                    lastMostPrice = oncePrices;	//更新单次的例如取最大值
                }
                else
                {
                    ans+=lastMostPrice;
                    minPrices = prices[i];		// 上面讨论的情况，在盈利有亏损的情况时就是我们累加的时候，然后接着将后面的当前的值看作一个新的开始
                    lastMostPrice = 0;
                }


            }
        }
        return ans+lastMostPrice;			//返回的时候记得加上最后一轮的累加值
```