- [十二.动态规划专题](#十二动态规划专题)
    - [70.爬楼梯](#70爬楼梯)
    - [62.不同路径](#62不同路径)
    - [63.不同路径2](#63不同路径2)
    - [剑指 Offer 13. 机器人的运动范围](#剑指-offer-13-机器人的运动范围)
    - [剑指 Offer 47. 礼物的最大价值](#剑指-offer-47-礼物的最大价值)
    - [64.最小路径和](#64最小路径和)
    - [120.三角形最小路径和](#120三角形最小路径和)
    - [118.杨辉三角](#118杨辉三角)
    - [119.杨辉三角2](#119杨辉三角2)
    - [44.通配符匹配](#44通配符匹配)
    - [10.正则表达式匹配](#10正则表达式匹配)
    - [97.交错字符串](#97交错字符串)
    - [87.扰乱字符串](#87扰乱字符串)
    - [115.不同的子序列](#115不同的子序列)
    - [72.编辑距离](#72编辑距离)
    - [剑指 Offer 46. 把数字翻译成字符串](#剑指-offer-46-把数字翻译成字符串)
    - [91.解码方法](#91解码方法)
    - [38.外观数列](#38外观数列)
    - [剑指 Offer 49. 丑数](#剑指-offer-49-丑数)
    - [剑指 Offer 62. 圆圈中最后剩下的数字](#剑指-offer-62-圆圈中最后剩下的数字)
    - [剑指 Offer 66. 构建乘积数组](#剑指-offer-66-构建乘积数组)


### 十二.动态规划专题


---------------------------------
##### 70.爬楼梯
>题目描述：假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/climbing-stairs


解题思路：经典动态规划题，dp[i] = dp[i-1] + dp[i-2]，可以用滚动数组的方式优化空间复杂度

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (1 == n)
            return 1;
        if (2 == n)
            return 2;
        int fx1 = 1, fx2 = 2;
        int fx;
        for (int i = 3; i <= n; i++)
        {
            fx = fx1 + fx2;
            fx1 = fx2;
            fx2 = fx;
        }
        return fx;
    }
};

```

<br>



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
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; ++i)
        {
            if (0 == obstacleGrid[i][0])
                dp[i][0] = 1;
            else
                break;
        }
        for (int j = 0; j < n; ++j)
        {
            if (0 == obstacleGrid[0][j])
                dp[0][j] = 1;
            else
                break;
        }
        for (int i = 1; i < m; ++i)
            for (int j = 1; j < n; ++j)
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

* **解法一**

解题思路：DFS，机器人相当于只能向下或者向右走，需要设置visited数组保存走过的路径，注意不需要回溯。

时间复杂度：O(M*N)

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

* **解法二**

解题思路：BFS，需要设置辅助visited队列和辅助queue

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp

class Solution {
public:
    int countK(int i)
    {
        int sum = 0;
        while (i)
        {
            sum += i % 10;
            i /= 10;
        }
        return sum;
    }
    
    int movingCount(int m, int n, int k) {
        int ans = 0;
        vector<vector<int>> visited(m, vector<int>(n, 0));
        queue<pair<int,int>> que;
        que.push(std::make_pair(0, 0));        
        visited[0][0] = 1;
        while (!que.empty())
        {
            queue<pair<int,int>> tQue;
            while(!que.empty())
            {
                pair<int,int> p = que.front();
                que.pop();
                ++ans;
                int i = p.first, j = p.second;
                if (i < m && j < n && countK(i+1) + countK(j) <= k && !visited[i+1][j])
                {
                    visited[i+1][j] = 1;
                    tQue.push(std::make_pair(i+1, j));
                }
                if (i < m && j < n && countK(i) + countK(j+1) <= k && !visited[i][j+1])
                {
                    visited[i][j+1] = 1;
                    tQue.push(std::make_pair(i, j+1));
                }
            }
            swap(que, tQue);
        }
        return ans;
    }
};

```


* **解法三**

解题思路：动态规划

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    int countK(int i)
    {
        int sum = 0;
        while (i)
        {
            sum += i % 10;
            i /= 10;
        }
        return sum;
    }

    int movingCount(int m, int n, int k) 
    {   
        if (k < 0)
            return 0;
        int cnt = 1;
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 1; i < m; ++i)
        {
            if (countK(i) > k)
                break;
            else
            {
                dp[i][0] = 1;
                ++cnt;
            }
        }
        for (int j = 1; j < n; ++j)
        {
            if (countK(j) > k)
                break;
            else
            {
                dp[0][j] = 1;
                ++cnt;
            }
        }
        for (int i = 1; i < m; ++i)
            for (int j = 1; j < n; ++j)
            {
                if ((0 == dp[i-1][j] && 0 == dp[i][j-1]) || countK(i) + countK(j) > k)
                    continue;
                else
                {
                    ++cnt;
                    dp[i][j] = 1;
                }
            }
        return cnt;
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

当然，该题也可以直接进行原地操作。

```cpp
int maxValue(vector<vector<int>>& grid) 
{   
    int m = grid.size(), n = grid[0].size();
    for (int i = 1; i < m; ++i)
        grid[i][0] += grid[i-1][0];
    for (int j = 1; j < n; ++j)
        grid[0][j] += grid[0][j-1];
    for (int i = 1; i < m; ++i)
        for (int j = 1; j < n; ++j)
            grid[i][j] = std::max(grid[i-1][j], grid[i][j-1]) + grid[i][j];
    return grid[m-1][n-1];
}
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

该题和上一题完全基本相同，也可以使用原地算法

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        for (int i = 1; i < m; ++i)
            grid[i][0] += grid[i-1][0];
        for (int j = 1; j < n; ++j)
            grid[0][j] += grid[0][j-1];
        for (int i = 1; i < m; ++i)
            for (int j = 1; j < n; ++j)
                grid[i][j] = std::min(grid[i-1][j], grid[i][j-1]) + grid[i][j];
        return grid[m-1][n-1];
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
        int n = triangle.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        dp[0][0] = triangle[0][0];
        for (int i = 1; i < n; ++i)
        {
            for (int j = 0; j <= i; ++j)
            {
                if (0 == j)
                    dp[i][j] = triangle[i][j] + dp[i-1][j];
                else if (i == j)
                    dp[i][j] = triangle[i][j] + dp[i-1][j-1];
                else
                    dp[i][j] = triangle[i][j] + std::min(dp[i-1][j-1], dp[i-1][j]);
            }
        }
        int ans = INT32_MAX;
        for (int j = 0; j <= n-1; ++j)
            ans = std::min(ans, dp[n-1][j]);
        return ans;
    }
};

```

在这里可以将空间复杂度进行优化一下，也就是用两个交替的dp数组，此时的空间复杂度为 O(N)

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<int>> dp(2, vector<int>(n, 0));
        dp[0][0] = triangle[0][0];
        for (int i = 1; i < n; ++i)
        {
            for (int j = 0; j <= i; ++j)
            {
                if (0 == j)
                    dp[1][j] = triangle[i][j] + dp[0][j];
                else if (i == j)
                    dp[1][j] = triangle[i][j] + dp[0][j-1];
                else
                    dp[1][j] = triangle[i][j] + std::min(dp[0][j], dp[0][j-1]);
            }
            dp[0] = dp[1];
        }
        int ans = INT32_MAX;
        for (int j = 0; j < n; ++j)
            ans = std::min(ans, dp[0][j]);
        return ans;
    }
};
```

<br>



---------------------------
##### 118.杨辉三角
>题目描述:给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。
在杨辉三角中，每个数是它左上方和右上方的数的和。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pascals-triangle

解题思路：杨辉三角的本质是动态规划，直接在for循环中创建数组再加入即可。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;
        for (int i = 0; i < numRows; i++)
        {
            vector<int> vec(i+1,  1);
            for (int j = 1; j < i; j++)
                vec[j] = ans[i-1][j] + ans[i-1][j-1];
            ans.push_back(std::move(vec));
        }
        return ans;
    }
};
```

<br>




---------------------------
##### 119.杨辉三角2
>题目描述:给定一个非负索引 k，其中 k ≤ 33，返回杨辉三角的第 k 行。
在杨辉三角中，每个数是它左上方和右上方的数的和。
进阶：
你可以优化你的算法到 O(k) 空间复杂度吗？
注意此题有一个坑，那就是k是从0开始取的，在程序中我们先将k+1即可。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pascals-triangle-ii

解题思路：动态规划优化法，用两个交替的数组即可。

时间复杂度：O(K^2)

空间复杂度：O(K)

```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        ++rowIndex;
        vector<vector<int>> dp(2, vector<int>(rowIndex, 1));
        for (int i = 1; i < rowIndex; ++i)
        {
            for (int j = 1; j < i; ++j)
                dp[1][j] = dp[0][j-1] + dp[0][j];
            dp[0] = dp[1];
        }
        return dp[0];
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

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        dp[0][0] = 1;
        for (int j = 1; j <= n; ++j)
            if ('*' == p[j-1])
                dp[0][j] = dp[0][j-1];
            
        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
            {
                if ('?' == p[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else if ('*' == p[j-1])
                    dp[i][j] = dp[i-1][j] || dp[i][j-1];
                else
                    dp[i][j] = (s[i-1] == p[j-1] && dp[i-1][j-1]);
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
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        dp[0][0] = 1;
        for (int j = 1; j <= n; ++j)
            if ('*' == p[j-1])
                dp[0][j] = dp[0][j-2];

        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
            {
                if ('.' == p[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else if ('*' == p[j-1])
                {
                    if (dp[i][j-2]) //匹配0个
                        dp[i][j] = dp[i][j-2];
                    else if (s[i-1]==p[j-2] || '.'==p[j-2])//匹配1个或多个
                        dp[i][j] = dp[i-1][j];
                }
                else
                    dp[i][j] = (s[i-1] == p[j-1] && dp[i-1][j-1]); 
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
状态转移方程为： dp[i][j] = (dp[i-1][j] && s1[i-1]==s3[i+j-1]) || (dp[i][j-1] && s2[j-1]==s3[i+j-1]);

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.size(), n = s2.size(), len = s3.size();
        if (m + n != len)
            return false;
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        dp[0][0] = 1;

        for (int i = 1; i <= m; ++i)
            dp[i][0] = (dp[i-1][0] && s1[i-1]==s3[i-1]);

        for (int j = 1; j <= n; ++j)
            dp[0][j] = (dp[0][j-1] && s2[j-1]==s3[j-1]);

        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
                dp[i][j] = (dp[i-1][j] && s1[i-1]==s3[i+j-1]) || (dp[i][j-1] && s2[j-1]==s3[i+j-1]);
    
        return dp[m][n];
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

* **解法一**

解题思路：递归，该方法的复杂度较高，会超时。

时间复杂度：

空间复杂度：

```cpp
class Solution {
public:
    bool isScramble(string s1, string s2) {
        if (s1 == s2)
            return true;
        string t1 = s1, t2 = s2;
        sort(t1.begin(), t1.end());
        sort(t2.begin(), t2.end());
        if (t1 != t2)
            return false;

        int n = s1.size();
        for (int i = 1; i < n; ++i)
        {
            bool ans1 = isScramble(s1.substr(0,i), s2.substr(0,i)) && isScramble(s1.substr(i,n-i), s2.substr(i,n-i));
            bool ans2 = isScramble(s1.substr(0,i), s2.substr(n-i,i)) && isScramble(s1.substr(i,n-i), s2.substr(0, n-i));
            if (ans1 || ans2)
                return true;
        }
        return false;
    }
};
```

* **解法二**

解题思路：使用三维动态规划的方法，dp[i][j][k]代表 s1以i为起点， s2以j为起点 ，长度为k 的字符串是否是可以通过交换得到。
设置len不断增加字符串的长度区间，用k在len中进行分隔，

时间复杂度：O(N^4)

空间复杂度：O(N^3)

```cpp
class Solution {
public:
    bool isScramble(string s1, string s2) {
        if (s1.size() != s2.size())
            return false;
        int n = s1.size();
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(n + 1, 0)));
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                dp[i][j][1] = (s1[i] == s2[j]);
        
        for (int len = 2; len <= n; ++len) //len是字符串的长度区间，逐步从2增加到n
            for (int i = 0; i <= n - len; ++i)
                for (int j = 0; j <= n - len; ++j)
                    for (int k = 1; k < len; ++k) //k作为分隔点，将长度区间len进行分隔
                    {
                        if (dp[i][j][k] && dp[i+k][j+k][len-k])//正常对应情况
                        {
                            dp[i][j][len] = true;
                            break;
                        }
                        if (dp[i][j+len-k][k] && dp[i+k][j][len-k])//前后交错对应情况
                        {
                            dp[i][j][len] = true;
                            break;
                        }
                    }

        return dp[0][0][n];
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
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下图所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
(上箭头符号 ^ 表示选取的字母)
rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/distinct-subsequences

解题思路：创建动态规划数组，dp[i][j]代表s[i]字符串中t[j]字符串出现的个数

时间复杂度：O(M*N)

空间复杂度：O(M*N)

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int m = s.size(), n = t.size();
        vector<vector<long>> dp(m+1, vector<long>(n+1, 0));
        dp[0][0] = 1;
        for (int i = 1; i <= m; ++i)
            dp[i][0] = 1;
        for (int j = 1; j <= n; ++j)
            dp[0][j] = 0;

        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
            {
                if (s[i-1] == t[j-1])
                    dp[i][j] = dp[i-1][j] + dp[i-1][j-1];
                else
                    dp[i][j] = dp[i-1][j];
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
        if ('0' == s[0])
            return 0;
        int dp0 = 1, dp1 = 1, dp2 = 1;
        for (int i = 1; i < s.size(); ++i)
        {
            if ('0' == s[i])
            {
                if (!('1' == s[i-1] || '2' == s[i-1]))
                    return 0;
                else
                    dp2 = dp0;
            }else
            {
                int num = stoi(s.substr(i-1, 2));
                if (10 < num && num <= 26)
                    dp2 = dp1 + dp0;
                else
                    dp2 = dp1;
            }
            dp0 = dp1, dp1 = dp2;
        }
        return dp2;
    }
};

```

<br>


-----------------------------
##### 38.外观数列
>题目描述：给定一个正整数 n ，输出外观数列的第 n 项。
「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。
你可以将其视作是由递归公式定义的数字字符串序列：
countAndSay(1) = "1"
countAndSay(n) 是对 countAndSay(n-1) 的描述，然后转换成另一个数字字符串。
前五项如下：
1.     1
2.     11
3.     21
4.     1211
5.     111221
第一项是数字 1 
描述前一项，这个数是 1 即 “ 一 个 1 ”，记作 "11"
描述前一项，这个数是 11 即 “ 二 个 1 ” ，记作 "21"
描述前一项，这个数是 21 即 “ 一 个 2 + 一 个 1 ” ，记作 "1211"
描述前一项，这个数是 1211 即 “ 一 个 1 + 一 个 2 + 二 个 1 ” ，记作 "111221"

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/count-and-say

解题思路：动态规划，dp[n] 总是通过 dp[n-1]计算得出，在此题中用两个string代替即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string countAndSay(int n) {
        string s0 = "1";
        for (int k = 2; k <= n; ++k)
        {
            string s1;
            int i = 0;
            while (i < s0.size())
            {
                int j = i;
                while (j < s0.size() && s0[j] == s0[i])
                    ++j;
                s1 = s1 + to_string(j - i) + s0[i];
                i = j;
            }
            s0 = s1;
        }
        return s0;
    }
};
```

也可以使用STL库中的find_if 和 find_if_not进行查找。

```cpp
class Solution {
public:
    string countAndSay(int n) {
        if (0 == n)
            return string();
        string s0 = "1";
        for (int i = 1; i < n; ++i)
        {
            auto beg = s0.begin();
            string s1;
            while (beg != s0.end())
            {
                auto it = std::find_if_not(beg, s0.end(), [&](char ch){
                    return *beg == ch;
                });
                int num = *beg - '0', cnt = std::distance(beg, it);
                s1 += to_string(cnt) + to_string(num);
                beg = it;
            }
            s0 = s1;
        }
        return s0;
    }
};

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
        int pos0 = 0, pos1 = 0;
        for (int i = 2; i <= n; ++i)
        {
            pos1 = (pos0 + m) % i;
            pos0 = pos1;
        }
        return pos0;
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
vector<int> constructArr(vector<int>& a)
{
    int n = a.size();
    vector<int> l(n, 1), r(n, 1);
    for (int i = 1; i < n; ++i)
        l[i] = l[i-1] * a[i-1];
    for (int i = n-2; i >= 0; --i)
        r[i] = r[i+1] * a[i+1];
    vector<int> ans(n, 0);
    for (int i = 0; i < n; ++i)
        ans[i] = l[i] * r[i];
    return ans;
}

```

<br>

