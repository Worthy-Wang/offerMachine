- [九.DFS专题](#九dfs专题)
        - [131.分割回文串](#131分割回文串)
        - [62.不同路径](#62不同路径)
        - [63.不同路径2](#63不同路径2)
        - [51.N皇后](#51n皇后)
        - [52.N皇后2](#52n皇后2)
        - [93.复原IP地址](#93复原ip地址)
        - [39.组合总合](#39组合总合)
        - [40.组合总合2](#40组合总合2)
        - [22.括号生成](#22括号生成)
        - [37.解数独](#37解数独)
        - [79.单词搜索](#79单词搜索)


# 九.DFS专题

---------------------------
##### 131.分割回文串
>题目描述:给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。
返回 s 所有可能的分割方案。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-partitioning

解题思路：DFS法 ，横向上遍历子串，在满足回文串的情况下再进行递归。

时间复杂度：

空间复杂度：

```cpp
class Solution {
    vector<vector<string>> ans;
public:
    bool isPalindrome(const string& s)
    {
        int l = 0, r = s.size()-1;
        while (l <= r)
        {
            if (s[l] != s[r])
                return false;
            l++,r--;
        }
        return true;
    }

    void DFS(string& s, int start , vector<string>& path)
    {
        if (start == s.size())
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i < s.size(); i++)
        {
            string sub = s.substr(start, i-start+1);
            if (!isPalindrome(sub))
                continue;
            path.push_back(sub);
            DFS(s, i + 1, path);
            path.pop_back();
        }
    }

    vector<vector<string>> partition(string s) {
        if (s.empty())
            return {};
        vector<string> path;
        DFS(s, 0, path);
        return ans;
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


---------------------------
##### 51.N皇后
>题目描述:n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。
上图为 8 皇后问题的一种解法。
给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。
每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。
皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/n-queens

解题思路：DFS回溯法，需要记录行，列，斜线的占用情况，当递归树的高度为i == n时进行返回。
每一个皇后会占用两条斜线，一条是 左上-右下 连线：横坐标-纵坐标为固定值；一条是 左下-右上 斜线：横坐标+纵坐标为固定值。 

时间复杂度：O(N!)

空间复杂度：O(N)

```cpp
class Solution {
    vector<vector<string>> ans;
public:
    void DFS(int n, int i, vector<string>& path, unordered_map<int,int>& columns, unordered_map<int,int>& rightSubLeft, unordered_map<int,int>& rightAddLeft)
    {
        if (i == n)
        {
            ans.push_back(path);
            return;
        }

        for (int j = 0; j < n; j++)
        {
            if (columns[j] || rightSubLeft[j-i] || rightAddLeft[j+i])
                continue;
            path[i][j] = 'Q';
            columns[j]=1 , rightSubLeft[j-i]=1, rightAddLeft[j+i]=1; 
            DFS(n, i+1, path, columns, rightSubLeft, rightAddLeft);
            path[i][j] = '.';
            columns[j]=0 , rightSubLeft[j-i]=0, rightAddLeft[j+i]=0; 
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        if (n <= 0)
            return {};
        vector<string> path(n, string(n, '.'));
        unordered_map<int,int> columns, rightSubLeft, rightAddLeft;
        DFS(n, 0, path, columns, rightSubLeft, rightAddLeft);
        return ans;
    }
};

```

<br>


---------------------------
##### 52.N皇后2
>题目描述:n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。
上图为 8 皇后问题的一种解法。
给定一个整数 n，返回 n 皇后不同的解决方案的数量。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/n-queens-ii

解题思路：与上一题的解法相同，只是不需要再设置path保存路径

时间复杂度：O(N!)

空间复杂度：O(N)

```cpp
class Solution {
    int ans = 0;
public:
    void DFS(int n, int i, unordered_map<int,int>& columns, unordered_map<int,int>& rightSubLeft, unordered_map<int,int>& rightAddLeft)
    {
        if (i == n)
        {
            ans++;
            return;
        }

        for (int j = 0; j < n; j++)
        {
            if (columns[j] || rightSubLeft[j-i] || rightAddLeft[j+i])
                continue;
            columns[j]=1 , rightSubLeft[j-i]=1, rightAddLeft[j+i]=1; 
            DFS(n, i+1, columns, rightSubLeft, rightAddLeft);
            columns[j]=0 , rightSubLeft[j-i]=0, rightAddLeft[j+i]=0; 
        }
    }

    int totalNQueens(int n) {
        if (n <= 0)
            return 0;
        unordered_map<int,int> columns, rightSubLeft, rightAddLeft;
        DFS(n, 0, columns, rightSubLeft, rightAddLeft);
        return ans;
    }
};

```

<br>


---------------------------
##### 93.复原IP地址
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>


---------------------------
##### 39.组合总合
>题目描述:给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的数字可以无限制重复被选取。
说明：
所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 
candidate 中的每个元素都是独一无二的。


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combination-sum

解题思路：

时间复杂度：

空间复杂度：

```cpp
18：20

```


<br>


---------------------------
##### 40.组合总合2
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>


---------------------------
##### 22.括号生成
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>


---------------------------
##### 37.解数独
>题目描述:编写一个程序，通过填充空格来解决数独问题。
一个数独的解法需遵循如下规则：
数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。
空白格用 '.' 表示。
答案被标成红色。
给定的数独序列只包含数字 1-9 和字符 '.' 。
你可以假设给定的数独只有唯一解。
给定数独永远是 9x9 形式的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sudoku-solver

解题思路：


时间复杂度：

空间复杂度：

```cpp
class Solution {
private:
    int rows[9][10] = {0};//rows[i][j] 表示第 i 行是否存在 j
    int cols[9][10] = {0};//cols[i][j] 表示第 i 列是否存在 j
    int grid[3][3][10] = {0};//grid[i][j][k] 表示第 {i, j} 个九宫格是否存在 k
    vector<pair<int, int>> spaces;//记录下空白格的坐标   
    vector<vector<char>> ans;
public:
    void dfs(vector<vector<char>>& board, int idx){
        //已经遍历完了空格，成功完成
        if(idx == spaces.size()){
            ans = board;
            return;
        }
        pair<int, int> axis = spaces[idx];
        int row = axis.first, col = axis.second;//当前空格的行和列
        //找到合法的可以填写的数字
        for(int i = 1; i <= 9; i ++){
            if(!rows[row][i] && !cols[col][i] && !grid[row/3][col/3][i]){
                board[row][col] = i + '0';
                rows[row][i] = 1; cols[col][i] = 1;
                grid[row/3][col/3][i] = 1;
                dfs(board, idx + 1);
                //回溯
                board[row][col] = '.';
                rows[row][i] = 0; cols[col][i] = 0;
                grid[row/3][col/3][i] = 0;
            }
        }
    }

    void solveSudoku(vector<vector<char>>& board) {
        //初始化
        for(int i = 0; i < 9; i ++){
            for(int j = 0; j < 9; j ++){
                if(board[i][j] == '.') spaces.push_back({i, j});
                else{
                    rows[i][board[i][j] - '0'] = 1;
                    cols[j][board[i][j] - '0'] = 1;
                    grid[i/3][j/3][board[i][j] - '0'] = 1;
                }
            }
        }
        dfs(board, 0);
        board = ans;
    }
};


```

<br>


---------------------------
##### 79.单词搜索
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

