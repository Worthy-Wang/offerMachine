## 目录
- [十一.贪心法专题](#十一贪心法专题)
    - [55.跳跃游戏](#55跳跃游戏)
    - [45.跳跃游戏2](#45跳跃游戏2)
    - [121.买卖股票的最佳时机](#121买卖股票的最佳时机)
    - [122.买卖股票的最佳时机2](#122买卖股票的最佳时机2)
    - [123.买卖股票的最佳时机3](#123买卖股票的最佳时机3)
    - [198. 打家劫舍](#198-打家劫舍)
    - [213. 打家劫舍 II](#213-打家劫舍-ii)


### 十一.贪心法专题

---------------------------
##### 55.跳跃游戏
>题目描述:给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
判断你是否能够到达最后一个位置。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jump-game

解题思路：贪心法，设置能够到达的最右边界即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int r = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            if (r < i)
                return false;
            r = std::max(r, i+nums[i]);
        }
        return true;
    }
};
```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 45.跳跃游戏2
>题目描述:给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
你的目标是使用最少的跳跃次数到达数组的最后一个位置。
假设你总是可以到达数组的最后一个位置。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jump-game-ii

解题思路：仍然是贪心法，在上一题的基础上设置每一次最长的跳跃极限，当达到该跳跃极限时，cnt++，并设置下一个跳跃的极限。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() == 1)
            return 0;
        int curEnd = nums[0], nextEnd = nums[0], cnt = 0;
        for (int i = 0; i < nums.size() - 1; ++i)
        {
            nextEnd = std::max(nextEnd, i + nums[i]);
            if (i == curEnd)
            {
                curEnd = nextEnd;
                cnt++;
            }
        }        
        return cnt + 1;
    }
};

```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 121.买卖股票的最佳时机 
>题目描述:给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
注意：你不能在买入股票前卖出股票。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock

解题思路：进行一次遍历，在遍历的过程中不断更新 最小值  最大利润 即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int lmin = prices[0];
        int ans = 0;
        for (int i = 1; i < prices.size(); ++i)
        {
            lmin = std::min(lmin, prices[i]);
            ans = std::max(prices[i] - lmin, ans);
        }
        return ans;
    }
};

```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 122.买卖股票的最佳时机2
>题目描述:给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii

解题思路：进行一次遍历，只要后面一天的价格大于前面一天的价格，立刻卖出，并将利润不断累加。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int ans = 0;
        for (int i = 1; i < prices.size(); i++)
            if (prices[i] > prices[i-1])
                ans += prices[i]-prices[i-1];
        return ans;
    }
};

```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 123.买卖股票的最佳时机3
>题目描述:给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。
注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii

解题思路：动态规划法，创建两个动态规划数组 left 与 right ，left记录 [0, i] 能够收获的最大收益， right记录 [i, n] 能够收获的最大收益；
left[i]：设置最小值，从左到右遍历，并存放得到的最大利润
right[i]：设置最大值，从右到左遍历，并存放得到的最大利润
最后找出left[i]+right[i]的最大值即可。 

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<int> l(n, 0), r(n, 0);
        int lmin = prices[0], rmax = prices[n-1];
        for (int i = 1; i < n; ++i){
            lmin = std::min(lmin, prices[i]);
            l[i] = std::max(l[i-1], prices[i] - lmin);            
        }   
        for (int i = n - 2; i >= 0; --i){
            rmax = std::max(rmax, prices[i]);
            r[i] = std::max(r[i+1], rmax - prices[i]);
        }
        int ans = 0;
        for (int i = 0 ; i < n; ++i)
            ans = std::max(ans, l[i] + r[i]);
        
        return ans;
    }
};

```

<br>



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 198. 打家劫舍
>题目描述：你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/house-robber

解题思路：动态规划，dp[i]代表到第i个房间能够偷到的最大金钱。dp[i] = std::max(dp[i-2] + nums[i], dp[i-1]);
其中动态规划师数组可以简化。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (0 == n)
            return 0;
        else if (1 == n)
            return nums[0];
        else
        {
            vector<int> dp(n, 0);
            int dp0 = nums[0];
            int dp1 = std::max(nums[0], nums[1]);
            int dp2 = dp1;
            for (int i = 2; i < n; ++i)
            {
                dp2 = std::max(dp0 + nums[i], dp1);
                dp0 = dp1;
                dp1 = dp2;
            }
            return dp2;
        }
    }
};

```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 213. 打家劫舍 II
>题目描述:你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。
给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/house-robber-ii

解题思路：该题解决方法和上一题相同，只是计算的时候需要把第一个和最后一个区分开即可。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    int robMax(vector<int>& nums, int start, int end)
    {
        vector<int> dp(end - start + 1, 0);
        int dp0 = nums[start];
        int dp1 = std::max(nums[start], nums[start + 1]);
        int dp2 = dp1;
        for (int i = start + 2; i <= end; ++i)
        {
            dp2 = std::max(dp0 + nums[i], dp1);
            dp0 = dp1;
            dp1 = dp2;
        }
        return dp2;
    }

    int rob(vector<int>& nums) {
        int n = nums.size();
        if (0 == n)
            return 0;
        else if (1 == n)
            return nums[0];
        else if (2 == n)
            return std::max(nums[0], nums[1]);
        else
        {
            return std::max(robMax(nums, 0, n-2), robMax(nums, 1, n-1));
        }
        
    }
};


```

<br>

<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


