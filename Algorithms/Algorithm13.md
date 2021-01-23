- [十三.动态规划专题](#十三动态规划专题)
        - [62.不同路径](#62不同路径)
        - [63.不同路径2](#63不同路径2)
        - [剑指 Offer 13. 机器人的运动范围](#剑指-offer-13-机器人的运动范围)
        - [剑指 Offer 47. 礼物的最大价值](#剑指-offer-47-礼物的最大价值)
        - [120.三角形最小路径和](#120三角形最小路径和)
        - [343. 整数拆分](#343-整数拆分)
        - [53.最大子序和](#53最大子序和)
        - [5.最长回文子串](#5最长回文子串)
        - [44.通配符匹配](#44通配符匹配)
        - [10.正则表达式匹配](#10正则表达式匹配)
        - [97.交错字符串](#97交错字符串)
        - [72.编辑距离](#72编辑距离)
        - [64.最小路径和](#64最小路径和)
        - [91.解码方法](#91解码方法)
        - [剑指 Offer 46. 把数字翻译成字符串](#剑指-offer-46-把数字翻译成字符串)
        - [115.不同的子序列](#115不同的子序列)
        - [139.单词拆分](#139单词拆分)
        - [140.单词拆分2](#140单词拆分2)
        - [剑指 Offer 49. 丑数](#剑指-offer-49-丑数)
        - [剑指 Offer 62. 圆圈中最后剩下的数字](#剑指-offer-62-圆圈中最后剩下的数字)
        - [123.买卖股票的最佳时机3](#123买卖股票的最佳时机3)
        - [剑指 Offer 66. 构建乘积数组](#剑指-offer-66-构建乘积数组)


# 十三.动态规划专题


---------------------------
##### 62.不同路径
>题目描述:一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。
问总共有多少条不同的路径？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-paths

解题思路：动态规划，dp[i][j] = dp[i-1][j] + dp[i][j-1] ，将dp数组的第一行与第一列都设置为1

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++)
            dp[i][0] = 1;
        for (int j = 0; j < n; j++)
            dp[0][j] = 1;
        for (int i = 1; i < m; i++)
            for (int j = 1; j < n; j++)
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
        return dp[m-1][n-1];
    }
};

```

<br>


---------------------------
##### 63.不同路径2
>题目描述:一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？
网格中的障碍物和空位置分别用 1 和 0 来表示。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-paths-ii

解题思路：仍然用上一题的dp动态规划法，将有障碍物的地方dp[i][j]=0即可，另外第一行与第一列在遇到障碍物之前dp为1，遇到之后为0

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        if (obstacleGrid.empty())
            return 0;
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 1));
        for (int i = 0; i < m; i++)
            if (1 == obstacleGrid[i][0])
            {
                for (int k = i; k < m; k++)
                    dp[k][0] = 0;
                break;
            }
        for (int j = 0; j < n; j++)
            if (1 == obstacleGrid[0][j])
            {
                for (int k = j; k < n; k++)
                    dp[0][k] = 0; 
                break;
            }
        for (int i = 1; i < m; i++)
            for (int j = 1; j < n; j++)
            {
                if (1 == obstacleGrid[i][j])
                    dp[i][j] = 0;
                else
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        return dp[m-1][n-1];
    }
};

```

<br>



---------------------------------------------
##### 剑指 Offer 13. 机器人的运动范围
>题目描述:地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof

解题思路：DFS，机器人相当于只能向下或者向右走，需要设置visited数组保存走过的路径，注意不需要回溯。

时间复杂度：O(M*N* 3^K) K是数字的位数

空间复杂度：O(M*N)

```cpp
class Solution {
    int ans = 0;
public:
    int count(int num)
    {
        int ans = 0;
        while (num)
        {
            ans += num % 10;
            num /= 10;
        }
        return ans;
    }

    void DFS(int m, int n, int i, int j, int k, vector<vector<int>>& visited)
    {
        if (i>=m || j>=n || count(i)+count(j)>k || visited[i][j])
            return;
        ans++;
        visited[i][j] = 1;
        DFS(m, n, i+1, j, k, visited);
        DFS(m, n, i, j+1, k, visited);
    }

    int movingCount(int m, int n, int k) {
        if (m<=0 || n<=0 || k<0)
            return 0;
        vector<vector<int>> visited(m, vector<int>(n, 0));
        DFS(m, n, 0, 0, k, visited);
        return ans;
    }
};
```

<br>

---------------------------
##### 剑指 Offer 47. 礼物的最大价值
>题目描述:在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof

解题思路：动态规划 dp[i][j] = std::max(dp[i-1][j],dp[i][j-1]) + grid[i][j], 该题也可以直接在原数组上进行修改。

时间复杂度：O(M * N)

空间复杂度：O(M * N)

```cpp
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        if (grid.empty())
            return 0;
        int m = grid.size(), n = grid[0].size(), ans = 0;
        vector<vector<int>> dp(m, vector<int>(n, 0));
        dp[0][0] = grid[0][0];
        
        for (int j = 1; j < n; j++)
            dp[0][j] = grid[0][j] + dp[0][j-1];
        for (int i = 1; i < m; i++)
            dp[i][0] = grid[i][0] + dp[i-1][0];
        for (int i = 1; i < m; i++)
            for (int j = 1; j < n; j++)
                dp[i][j] = std::max(dp[i-1][j], dp[i][j-1]) + grid[i][j];
        return dp[m-1][n-1];
    }
};

```

<br>


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




-----------------------------
##### 5.最长回文子串
>题目描述：给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-palindromic-substring

* **解法一**

解题思路：暴力法，用两层for循环O(N^2)，再使用一层for循环判断是否为回文。

时间复杂度：O(N^3)

空间复杂度：O(1)

* **解法二**

解题思路：动态规划，dp[i][j],i代表起始位置，j代表结束位置。二维动态数组只需要填充右上部分，并且是从左往右一列一列的填充。
状态转移方程：dp[i][j] = (s[i]==s[j] && dp[i+1][j-1])

时间复杂度：O(N^2)

空间复杂度：O(N^2)

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int maxLen = 0;
        pair<int,int> res;
        int N = s.size();
        vector<vector<int>> dp(N, vector<int>(N, 0));
        for (int j = 0; j < N; j++)
        {
            for (int i = 0; i <= j; i++)
            {
                if (i == j)
                    dp[i][j] = 1;
                else if(j-i==1 || j-i==2)
                    dp[i][j] = (s[i]==s[j]);
                else
                    dp[i][j] = (dp[i+1][j-1]&&s[i]==s[j]);
                
                if (dp[i][j] && maxLen<j-i+1)
                {
                    maxLen = j-i+1;
                    res.first = i, res.second = j;
                }
            }
        } 
        int i = res.first, j = res.second;
        return s.substr(i, j-i+1);
    }
};

```


* **解法三**

解题思路：中心扩展法，以一个for循环遍历元素，该元素作为回文子串的中心左右两边扩散。注意需要区分最长回文的子串有奇数个还是偶数个。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.empty())
            return string();
        string ans;
        int maxLen = 0;
        for (int i = 0; i < s.size(); i++)
        {
            //回文串字符数为奇数
            int l = i, r = i;
            while (0<=l&&r<s.size() && s[l]==s[r])
                l--, r++;
            if (r-l-1 > maxLen)
            {
                maxLen = r - l - 1;
                ans = s.substr(l+1, maxLen);
            }

            //回文串字符数为偶数
            l = i, r = i+1;
            while (0<=l&&r<s.size() && s[l]==s[r])
                l--, r++;
            if (r-l-1 > maxLen)
            {
                maxLen = r - l - 1;
                ans = s.substr(l+1, maxLen);
            }
        }
        return ans;
    }
};

```


<br>




-----------------------------
##### 44.通配符匹配
>题目描述：给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '*' 的通配符匹配。
'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
两个字符串完全匹配才算匹配成功。
说明:
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 *。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/wildcard-matching

解题思路：动态规划，dp[i][j]代表s[i]长度的字符串能否被p[j]长度的字符串进行通配符匹配
s[i]==p[j] dp[i][j] = dp[i-1][j-1]
'?'==p[j] dp[i][j] = dp[i-1][j-1]
'*'==p[j] dp[i][j] = dp[i-1][j] || dp[i][j-1]

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
        dp[0][0] = true;
        for (int j = 1; j <= n; ++j)
            if ('*' == p[j-1])
                dp[0][j] = dp[0][j-1];

        for (int i = 1; i <= m; ++i )
            for (int j = 1; j <= n; ++j)
            {
                if ('*' == p[j-1])
                {
                    if (dp[i][j-1]) //匹配0个
                        dp[i][j] = true;
                    else //匹配1个或多个
                        dp[i][j] = dp[i-1][j];
                }else if ('?' == p[j-1])
                {
                    dp[i][j] = dp[i-1][j-1];
                }else
                {
                    if (s[i-1] == p[j-1])
                        dp[i][j]  = dp[i-1][j-1];
                    else
                        dp[i][j] = false;
                }
            }
        return dp[m][n];
    }
};

```

<br>


-----------------------------
##### 10.正则表达式匹配
>题目描述：给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/regular-expression-matching

解题思路：动态规划，dp[i][j]代表s[i]为止的串能够被p[j]为止的串用正则表达式匹配。首先对二维动态规划的数组多扩充一行一列用来存储空字符串，完善初始状态；然后开始遍历数组，先看s[i] 与 p[j]是否相同（p[j]是'.'也可以），在根据 p[j]是 '*'的情况进行判断，*可以代表0个或者多个，就比较复杂了。

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
        dp[0][0] = true;
        for (int j = 1; j <= n; ++j)
            if  ('*' == p[j-1])
                dp[0][j] = dp[0][j-2];

        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
            {
                if ('*' == p[j-1])
                {
                    if (dp[i][j-2]) //*匹配0个
                        dp[i][j] = true;
                    else if (s[i-1]==p[j-2] || '.'==p[j-2])//*匹配1个或多个
                        dp[i][j] = dp[i-1][j];
                }else if('.' == p[j-1])
                {
                    dp[i][j] = dp[i-1][j-1];
                }else
                {
                    if (s[i-1] == p[j-1])
                        dp[i][j] = dp[i-1][j-1];
                    else
                        dp[i][j] = false;
                }
            }
        return dp[m][n];  
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

解题思路：dp[i][j]代表s1[i]的子串和s2[j]的子串能否形成交错的字符串。

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.size(), n = s2.size();
        if (m + n != s3.size())
            return false;
        
        vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
        dp[0][0] = true;
        for (int i = 1; i <= m; ++i)
            if (s1[i-1] == s3[i-1])
                dp[i][0] = true;
            else
                break;

        for (int j = 1; j <= n; ++j)
            if (s2[j-1] == s3[j-1])
                dp[0][j] = true;
            else
                break;
        
        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
            {
                if ((dp[i-1][j]&&s1[i-1]==s3[i+j-1]) || (dp[i][j-1]&&s2[j-1]==s3[i+j-1]))
                    dp[i][j] = true;
                else
                    dp[i][j] = false;
            }
        return dp[m][n];
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

解题思路：动态规划，先处理遇到 00 10 20 30 ... 的情况，这几种情况中只有 10 20 是合法的。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int numDecodings(string s) {
        if (s.empty() || '0' == s[0])
            return 0;
        int dp0 = 1, dp1 = 1, dp2 = 1;
        for (int i = 1; i < s.size(); i++)
        {
            if ('0' == s[i])
            {
                if ('1'==s[i-1] || '2'==s[i-1])
                    dp2 = dp0;
                else
                    return 0;
            }else
            {
                if (('2'==s[i-1]&&'1'<=s[i]&&s[i]<='6') || ('1'==s[i-1]))
                    dp2 = dp1 + dp0;
                else
                    dp2 = dp1;
            }
            dp0 = dp1;
            dp1 = dp2;
        }
        return dp2;
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


时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int translateNum(int num) {
        int dp0 = 1, dp1 = 1, dp2 = 1;
        string s(to_string(num));
        for (int i = 1; i < s.size(); i++)
        {
            int x = stoi(s.substr(i-1, 2));
            if (10<=x && x<=25)
                dp2 = dp1 + dp0;
            else
                dp2 = dp1;
            dp0 = dp1;
            dp1 = dp2;
        }        
        return dp2;
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



---------------------------
##### 剑指 Offer 49. 丑数
>题目描述:我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/chou-shu-lcof

解题思路：动态规划，在已经是丑数的基础上*2 *3 *5 ,那么下一个数也一定是丑数，再比较这几个相乘出来的数哪一个小一点，那么就是下一个丑数。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> dp(n+1);
        dp[0] = 0, dp[1] = 1;
        int i = 1, j = 1, k = 1;
        for (int idx = 2; idx <= n; ++idx)
        {
            dp[idx] = std::min(2*dp[i], std::min(3*dp[j], 5*dp[k]));
            if (dp[idx] == 2*dp[i])
                i++;
            if (dp[idx] == 3*dp[j])
                j++;
            if (dp[idx] == 5*dp[k])
                k++;
        }
        return dp[n];
    }
};
```

<br>





---------------------------
##### 剑指 Offer 62. 圆圈中最后剩下的数字
>题目描述:0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。
例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof

解题思路：经典的约瑟夫问题，采用动态规划去完成。
pos          n
0            1
(0+m)%2      2
dp1 = (dp0+m)%n
     
时间复杂度：O(N)
  
空间复杂度：O(1)

```cpp
class Solution {
public:
    int lastRemaining(int n, int m) {
        if (0 == n || 1 == n)
            return 0;
        int dp0 = 0, dp1;
        for (int i = 1; i <= n; i++)
        {
            dp1 = (dp0 + m) % i;
            dp0 = dp1;
        }
        return dp1;
    }
};
```

<br>



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
        vector<int> left(n), right(n);
        left[0] = 0, right[n-1] = 0;
        int minPrice = prices[0], maxPrice = prices[n-1];
        for (int i = 1; i < n; i++)
        {
            minPrice = std::min(minPrice, prices[i]);
            left[i] = std::max(left[i-1], prices[i]-minPrice);
        }
        for (int i = n-2; i >= 0; i-- )
        {
            maxPrice = std::max(prices[i], maxPrice);
            right[i] = std::max(right[i+1], maxPrice - prices[i]);         
        }
        int ans = 0;
        for (int i = 0; i < n; i++)
            ans = std::max(left[i]+right[i], ans);
        return ans;
    }
};

```

<br>






---------------------------
##### 剑指 Offer 66. 构建乘积数组
>题目描述:给定一个数组 A[0,1,…,n-1]，请构建一个数组 B[0,1,…,n-1]，其中 B 中的元素 B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof

解题思路：动态规划，构建left 和 right 两个动态规划数组保存 累乘的积。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        if (a.empty())
            return {};
        int n = a.size();
        vector<int> left(n), right(n);
        left[0] = 1, right[n-1] = 1;
        for (int i = 1; i < n; i++)
            left[i] = left[i-1] * a[i-1];
        for (int i = n-2; i >= 0; i--)
            right[i] = right[i+1] * a[i+1];
        vector<int> ans;
        for (int i = 0; i < n; i++)
            ans.push_back(left[i]*right[i]);
        return ans;
    }
};

```

<br>

