- [十二.动态规划专题](#十二动态规划专题)
        - [120.三角形最小路径和](#120三角形最小路径和)
        - [343. 整数拆分](#343-整数拆分)
        - [53.最大子序和](#53最大子序和)
        - [84.柱状图中最大的矩形](#84柱状图中最大的矩形)
        - [85.最大矩阵](#85最大矩阵)
        - [97.交错字符串](#97交错字符串)
        - [87.扰乱字符串](#87扰乱字符串)
        - [64.最小路径和](#64最小路径和)
        - [72.编辑距离](#72编辑距离)
        - [91.解码方法](#91解码方法)
        - [剑指 Offer 46. 把数字翻译成字符串](#剑指-offer-46-把数字翻译成字符串)
        - [115.不同的子序列](#115不同的子序列)
        - [139.单词拆分](#139单词拆分)
        - [140.单词拆分2](#140单词拆分2)


# 十二.动态规划专题

---------------------------
##### 120.三角形最小路径和
>题目描述:给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。
相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。
如果你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题，那么你的算法会很加分。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/triangle

解题思路：动态规划，创建一个和原三角形相同大小的数组，dp[i][j] = triangle[i][j] + std::min(dp[i-1][j], dp[i-1][j-1])，然后遍历最后一层找出最小啊的那一个即可。

时间复杂度：O(N^2)

空间复杂度：O(N^2)

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int N = triangle.size();
        vector<vector<int>> dp(N, vector<int>(N, 0));
        dp[0][0] = triangle[0][0];
        for (int i = 1; i < N; i++)
        {
            dp[i][0] = triangle[i][0]+dp[i-1][0];//每行的第一个数单独处理
            for (int j = 1; j < i; j++)
                dp[i][j] = triangle[i][j] + std::min(dp[i-1][j-1], dp[i-1][j]);
            dp[i][i] = triangle[i][i] + dp[i-1][i-1];//每行的最后一个数单独处理
        }
        int ans = INT32_MAX;
        for (int j = 0; j < N; j++)
            ans = std::min(ans, dp[N-1][j]);
        return ans;
    }
};

```

在这里可以将空间复杂度进行优化一下，也就是用两个交替的dp数组，此时的空间复杂度为 O(N)

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int N = triangle.size();
        vector<vector<int>> dp(2, vector<int>(N, 0));
        dp[0][0] = triangle[0][0];
        for (int i = 1; i < N; i++)
        {
            dp[1][0] = triangle[i][0]+dp[0][0];//每行的第一个数单独处理
            for (int j = 1; j < i; j++)
                dp[1][j] = triangle[i][j] + std::min(dp[0][j-1], dp[0][j]);
            dp[1][i] = triangle[i][i] + dp[0][i-1];//每行的最后一个数单独处理
            swap(dp[0], dp[1]);
        }

        int ans = INT32_MAX;
        for (int j = 0; j < N; j++)
            ans = std::min(ans, dp[0][j]);
        return ans;
    }
};

```

<br>





---------------------------
##### 343. 整数拆分
>题目描述：给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/integer-break

解题思路：动态规划

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n+1, 0);
        dp[0] = 0, dp[1] = 1;
        for (int i = 2; i <= n; i++)
            for (int j = 1; j < i; j++)
                dp[i] = std::max(dp[i], std::max(j*(i-j), dp[j]*(i-j)));
        return dp[n];
    }
};

```

<br>



---------------------------
##### 53.最大子序和
>题目描述:给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-subarray

解题思路：动态规划，dp[i] = std::max(dp[i-1]+nums[i], nums[i]), 将动态规划数组优化为只有两个即 dp[0] 和 dp[1]

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.empty())
            return 0;
        vector<int> dp(2);
        dp[0] = nums[0];
        int maxLen = nums[0];
        for (int i =1; i < nums.size(); i++)
        {
            dp[1] = std::max(dp[0]+nums[i], nums[i]);
            maxLen  = std::max(dp[1], maxLen);
            swap(dp[0], dp[1]);
        }
        return maxLen;
    }
};

```

<br>


-------------------------------------
##### 84.柱状图中最大的矩形
>题目描述:给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
求在该柱状图中，能够勾勒出来的矩形的最大面积。
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/largest-rectangle-in-histogram

解题思路：首先想到暴力法，也可以说是中心扩散法；再在暴力法的基础上进行演化，使用单调栈法，栈顶元素作为最大值，可以找到左右两边比它小的元素。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        if (heights.empty())
            return 0;
        heights.push_back(0);
        int ans = 0;
        stack<int> stk;
        for (int i = 0; i < heights.size(); i++)
        {
            while (!stk.empty() && heights[i]<heights[stk.top()])
            {
                int mid = stk.top();
                stk.pop();
                int l = stk.empty()? -1 : stk.top();
                int r = i;
                ans = std::max(ans, (r-l-1)*heights[mid]);
            }
            stk.push(i);
        }
        return ans;
    }
};

```

<br>


---------------------------
##### 85.最大矩阵
>题目描述:给定一个仅包含 0 和 1 、大小为 rows x cols 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximal-rectangle

* **解法一**

解题思路：调用上一题求最大矩阵的解法，一行一行的计算，求解出最大矩阵

时间复杂度：O(M*N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        if (heights.empty())
            return 0;
        heights.push_back(0);
        int ans = 0;
        stack<int> stk;
        for (int i = 0; i < heights.size(); i++)
        {
            while (!stk.empty() && heights[i]<heights[stk.top()])
            {
                int mid = stk.top();
                stk.pop();
                int l = stk.empty()? -1 : stk.top();
                int r = i;
                ans = std::max(ans, (r-l-1)*heights[mid]);
            }
            stk.push(i);
        }
        heights.pop_back();
        return ans;
    }

    int maximalRectangle(vector<vector<char>>& matrix) {
        if (matrix.empty())
            return 0;
        int ans = 0;
        int M = matrix.size(), N = matrix[0].size();
        vector<vector<int>> nums(M, vector<int>(N));
        for (int i = 0; i < M; i++)
            for (int j = 0; j < N; j++)
                if ('1' == matrix[i][j])
                    nums[i][j] = 1;
                else 
                    nums[i][j] = 0;

        for (int i = 1; i < M; i++)
            for (int j = 0; j < N; j++)
                if (0 == nums[i][j])
                    continue;
                else
                    nums[i][j] += nums[i-1][j];
                    
        for (int i = 0; i < M; i++)
        {
            int res = largestRectangleArea(nums[i]);
            ans = std::max(ans, res);
        }
        return ans;
    }
};

```


<br>


---------------------------
##### 97.交错字符串
>题目描述:给定三个字符串 s1、s2、s3，请你帮忙验证 s3 是否是由 s1 和 s2 交错 组成的。
两个字符串 s 和 t 交错 的定义与过程如下，其中每个字符串都会被分割成若干 非空 子字符串：
s = s1 + s2 + ... + sn
t = t1 + t2 + ... + tm
|n - m| <= 1
交错 是 s1 + t1 + s2 + t2 + s3 + t3 + ... 或者 t1 + s1 + t2 + s2 + t3 + s3 + ...
提示：a + b 意味着字符串 a 和 b 连接。
s1、s2、和 s3 都由小写英文字母组成


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/interleaving-string

解题思路：动态规划法，类似于机器人走路径的那一题，判断是否能够走到最后一个格子即可。
dp[i][j] = (dp[i-1][j]&&s3[i+j-1]==s1[i-1]) || (dp[i][j-1]&&s3[i+j-1]==s2[j-1]);
  0 a a b   s2 j
0 T F F F
a T 
b T
c T
s1
i

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        if (s1.size() + s2.size() != s3.size())
            return false;
        int M = s1.size(), N = s2.size();
        vector<vector<bool>> dp(M+1, vector<bool>(N+1, false));
        dp[0][0] = true;
        for (int j = 1; j <= N; j++)
        {
            if (s3[j-1]==s2[j-1])
                dp[0][j] = true;
            else
                break;
        }
        for (int i = 1; i <= M; i++)
        {
            if (s3[i-1]==s1[i-1])
                dp[i][0] = true;
            else
                break;
        }

        for (int i = 1; i <= M; i++)
            for(int j = 1; j <= N; j++)
                dp[i][j] = (dp[i-1][j]&&s3[i+j-1]==s1[i-1]) || (dp[i][j-1]&&s3[i+j-1]==s2[j-1]);
        return dp[M][N];
    }
};
```

<br>


---------------------------
##### 87.扰乱字符串
>题目描述:给定一个字符串 s1，我们可以把它递归地分割成两个非空子字符串，从而将其表示为二叉树。
下图是字符串 s1 = "great" 的一种可能的表示形式。
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
在扰乱这个字符串的过程中，我们可以挑选任何一个非叶节点，然后交换它的两个子节点。
例如，如果我们挑选非叶节点 "gr" ，交换它的两个子节点，将会产生扰乱字符串 "rgeat" 。
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
我们将 "rgeat” 称作 "great" 的一个扰乱字符串。
同样地，如果我们继续交换节点 "eat" 和 "at" 的子节点，将会产生另一个新的扰乱字符串 "rgtae" 。
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
我们将 "rgtae” 称作 "great" 的一个扰乱字符串。
给出两个长度相等的字符串 s1 和 s2，判断 s2 是否是 s1 的扰乱字符串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/scramble-string

解题思路：

时间复杂度：

空间复杂度：

```cpp

```

<br>


---------------------------
##### 64.最小路径和
>题目描述:给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
说明：每次只能向下或者向右移动一步。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-path-sum

解题思路：动态规划，dp[i][j] = std::min(dp[i-1][j], dp[i][j-1]), 第一行，第一列的值分别不断累加。

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if (grid.empty())
            return 0;
        int M = grid.size(), N = grid[0].size();
        vector<vector<int>> dp(M, vector<int>(N, 0));
        dp[0][0] = grid[0][0];
        for (int j = 1; j < N; j++)
            dp[0][j] = grid[0][j] + dp[0][j-1];
        for (int i = 1; i < M; i++)
            dp[i][0] = grid[i][0] + dp[i-1][0];
        
        for (int i = 1; i < M; i++)
            for (int j = 1; j < N; j++)
                dp[i][j] = std::min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
        return dp[M-1][N-1];
    }
};

```

<br>




---------------------------
##### 72.编辑距离
>题目描述:给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。word1 和 word2 由小写英文字母组成
你可以对一个单词进行如下三种操作：
插入一个字符
删除一个字符
替换一个字符
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/edit-distance

解题思路：dp[i][j]存放的是转换到达该位置需要的步骤，i向下代表 删除， j向右代表 增加，ij同时增加代表修改；整个动态规划数组执行到右下角的时候，已经将word1全部删除，将Word2全部加入，达到最后的修改结果，求解出最少的编辑距离。
word[i]!=word[i] 时 dp[i][j] = std::min(dp[i-1][j-1], std::min(dp[i-1][j],dp[i][j-1])) + 1; 
当word[i]==word[j]时 dp[i][j] = dp[i-1][j-1];
    r o s s
  0 1 2 3 4
r 1
i 2
c 3
e 4

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int M  = word1.size(), N = word2.size();
        vector<vector<int>> dp(M+1, vector<int>(N+1, 0));
        dp[0][0] = 0;
        for (int i = 1; i <= M; i++)
            dp[i][0] = i;
        for (int j = 1; j <= N; j++)
            dp[0][j] = j;
        
        for (int i = 1; i <= M; i++)
            for (int j = 1; j <= N;j ++)
                if (word1[i-1]==word2[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else
                    dp[i][j] = std::min(dp[i-1][j-1], std::min(dp[i-1][j],dp[i][j-1])) + 1;
        return dp[M][N];
    }
};

```

<br>




---------------------------
##### 91.解码方法
>题目描述:一条包含字母 A-Z 的消息通过以下方式进行了编码：
'A' -> 1
'B' -> 2
...
'Z' -> 26
给定一个只包含数字的非空字符串，请计算解码方法的总数。
题目数据保证答案肯定是一个 32 位的整数。
s 只包含数字，并且可能包含前导零。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/decode-ways

解题思路：动态规划，先处理遇到 00 10 20 30 的情况

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int numDecodings(string s) {
        if (s.empty() || s[0]=='0')
            return 0;
        int n = s.size();
        unordered_map<int,int> dp;
        dp[-1] = 1, dp[0] = 1;
        for (int i = 1; i < n; i++)
        {
            int num = stoi(s.substr(i-1,2));
            if ('0' == s[i]) // 00 10 20 30 40...
            {
                if (s[i-1]=='0' || s[i-1]>='3')
                    return 0;
                else 
                    dp[i] = dp[i-2];
            }
            else if (11<=num && num<= 26) // 11~26 
                dp[i] = dp[i-1] + dp[i-2];
            else //27 28...
                dp[i] = dp[i-1];
        }
        return dp[n-1];
    }
};

```

<br>





---------------------------
##### 剑指 Offer 46. 把数字翻译成字符串
>题目描述:给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof

解题思路：动态规划，该题 相较于上一题更为简单，因为0可以直接翻译。
0~9 : dp[i] = dp[i-1];
10~25: dp[i] = dp[i-1] + dp[i-2];
26~... : dp[i] = dp[i-1];

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int translateNum(int num) {
        string s = to_string(num);
        int n = s.size();
        unordered_map<int,int> dp;
        dp[-1] = 1, dp[0] = 1;
        for (int i = 1; i < n ; i++)
        {
            if ('0' == s[i-1]) // 0~9
                dp[i] = dp[i-1];
            else if ('1'==s[i-1] || ('2'==s[i-1]&&'0'<=s[i]&&s[i]<='5')) // 10~25
                dp[i] = dp[i-1] + dp[i-2];
            else //26...
                dp[i] = dp[i-1];
        }
        return dp[n-1];
    }
};

```

<br>


---------------------------
##### 115.不同的子序列
>题目描述:给定一个字符串 s 和一个字符串 t ，计算在 s 的子序列中 t 出现的个数。
字符串的一个 子序列 是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）
题目数据保证答案符合 32 位带符号整数范围。
s 和 t 由英文字母组成

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/distinct-subsequences

解题思路：创建动态规划数组，将字符串t的子串不断在s串中进行匹配，最后生成动态规划数组。
if (s[j] == t[i]) dp[i][j] = dp[i][j-1] + dp[i-1][j-1] 
if (s[j] != t[i]) dp[i][j] = dp[i][j-1];

  "" a b c b c  s j
""1  1 1 1 1 1 
b 0  0 1 1 2 2
c 0  0 0 1 1 3

t 
i

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int N = s.size(), M = t.size();
        vector<vector<long>> dp(M+1, vector<long>(N+1, 0));
        dp[0][0] = 1;
        for (int j = 1; j <= N; j++)
            dp[0][j] = 1;
        for (int i = 1; i <= M; i++)
            dp[i][0] = 0;
        
        for (int i = 1; i <= M; i++)
        {
            for (int j = 1; j <= N; j++)
            {
                if (s[j-1] == t[i-1]) 
                    dp[i][j] = dp[i][j-1] + dp[i-1][j-1];
                else
                    dp[i][j] = dp[i][j-1];
            }
        }
        return dp[M][N];
    }
};


```

<br>




---------------------------
##### 139.单词拆分
>题目描述:给定一个非空字符串 s 和一个包含非空单词的列表 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。
说明：
拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-break

解题思路：动态规划，创建动态规划数组存放哪些位置可以被拆分。

时间复杂度：O(M + N^2) N是s的长度，M是wordDict的长度 

空间复杂度：O(M+N)

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> hashset(wordDict.begin(), wordDict.end());
        vector<bool> dp(s.size()+1);
        dp[0] = true;
        for (int j = 1; j <= s.size(); j++)
        {
            for (int i = 0; i < j; i++)
            {
                if (dp[i] && hashset.count(s.substr(i, j-i)))
                {
                    dp[j] = true;
                    break;
                }
            }
        }        
        return dp[s.size()];
    }
};

```

<br>




---------------------------
##### 140.单词拆分2
>题目描述:给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。
说明：
分隔时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-break-ii

解题思路：

时间复杂度：

空间复杂度：

```cpp

```

<br>


