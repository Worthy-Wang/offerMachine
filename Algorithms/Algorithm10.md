- [十.DFS专题](#十dfs专题)
        - [131.分割回文串](#131分割回文串)
        - [132.分割回文串2](#132分割回文串2)
        - [剑指 Offer 13. 机器人的运动范围](#剑指-offer-13-机器人的运动范围)
        - [51.N皇后](#51n皇后)
        - [52.N皇后2](#52n皇后2)
        - [93.复原IP地址](#93复原ip地址)
        - [39.组合总合](#39组合总合)
        - [40.组合总合2](#40组合总合2)
        - [22.括号生成](#22括号生成)
        - [37.解数独](#37解数独)
        - [79.单词搜索](#79单词搜索)


# 十.DFS专题

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
##### 132.分割回文串2
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>



---------------------------------------------
##### 剑指 Offer 13. 机器人的运动范围
>题目描述:地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof

解题思路：DFS，机器人相当于只能向下或者向右走，需要设置visited数组保存走过的路径，注意不需要回溯。

时间复杂度：O(M*N* K) K是数字的位数

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
>题目描述:给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。
有效的 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。
例如："0.1.2.201" 和 "192.168.1.1" 是 有效的 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效的 IP 地址。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/restore-ip-addresses

解题思路：DFS法，设置path辅助变量存储分割好的数字，设置start起始位置作为每一次数字的开头，剪枝操作实质上也就是将不符合规定的IP地址值跳过。

时间复杂度：O(N!)

空间复杂度：O(N)

```cpp
class Solution {
    vector<string> ans;
public:
    void DFS(string& s, int start, vector<string>& path)
    {
        if (4==path.size() && start==s.size())
        {
            ans.push_back(string(path[0]+"."+path[1]+"."+path[2]+"."+path[3]));
            return;
        }

        for (int j = start; j < s.size(); j++)
        {
            string str = s.substr(start, j-start+1);
            if (stoi(str)>255)
                break;
            path.push_back(str);
            DFS(s, j+1, path);  
            path.pop_back();  
            if ("0" == str)
                break;
        }
    }

    vector<string> restoreIpAddresses(string s) {
        if (s.size() > 12)
            return {};
        vector<string> path;
        DFS(s, 0, path);
        return ans;
    }
};

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

解题思路：DFS法，需要设置辅助变量path记录路径，以及每次遍历的起始点start。

时间复杂度：O(N!)

空间复杂度：O(N)

```cpp
class Solution {
    vector<vector<int>> ans;
public:
    void DFS(vector<int>& candidates, int start, int target, vector<int>& path)
    {
        if (target < 0)
            return;
        if (target==0)
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i < candidates.size(); i++)
        {
            path.push_back(candidates[i]);
            DFS(candidates, i, target-candidates[i], path);
            path.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        if (candidates.empty())
            return {};
        vector<int> path;
        DFS(candidates, 0, target, path);
        return ans;
    }
};

```


<br>


---------------------------
##### 40.组合总合2
>题目描述:给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的每个数字在每个组合中只能使用一次。
所有数字（包括目标数）都是正整数。
解集不能包含重复的组合。 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combination-sum-ii

解题思路：用DFS法解决，需要设置辅助变量path记录路径，并设置起始点start。

时间复杂度：O(N!)

空间复杂度：O(N)

```cpp
class Solution {
    vector<vector<int>> ans;
public:
    void DFS(vector<int>& candidates, int target, int start,  vector<int>& path)
    {
        if (target < 0)
            return;
        if (0 == target)
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i < candidates.size(); i++)
        {
            if (i>start && candidates[i]==candidates[i-1])
                continue;
            if (candidates[i] > target)
                return;
            path.push_back(candidates[i]);
            DFS(candidates, target-candidates[i], i+1, path);
            path.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        if (candidates.empty())
            return {};
        vector<int> path;
        sort(candidates.begin(), candidates.end());
        DFS(candidates, target, 0, path);
        return ans;
    }
};


```

<br>


---------------------------
##### 22.括号生成
>题目描述:数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/generate-parentheses

解题思路：DFS法，记录左右括号的剩余值，当剩余值都为0时说明匹配完成；注意左括号的剩余值是必须小于右括号的剩余值。

时间复杂度：

空间复杂度：

```cpp
class Solution {
    vector<string> ans;
public:
    void DFS(int l, int r, string& path)
    {
        if (0==l && 0==r)
        {
            ans.push_back(path);
            return;
        }

        if (l)
        {
            path.push_back('(');
            DFS(l-1, r, path);
            path.pop_back();
        }
        if (l < r)
        {
            path.push_back(')');
            DFS(l, r-1, path);
            path.pop_back();
        }
    }

    vector<string> generateParenthesis(int n) {
        if (n <= 0)
            return {};
        string path;
        DFS(n, n, path);
        return ans;
    }
};

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
>题目描述:给定一个二维网格和一个单词，找出该单词是否存在于网格中。
单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。
board 和 word 中只包含大写和小写英文字母。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-search

解题思路：DFS，设置辅助visited数组记录走过的路径，设置 idx 作为word的 下标。

时间复杂度：O(M*N*3^L) L为word的长度

空间复杂度：O(M*N+L)

```cpp
class Solution {
public:
    bool DFS(vector<vector<char>>& board, int i, int j, int M, int N,  vector<vector<int>>& visited, string& word ,int idx)
    {
        if (idx == word.size())
            return true;
        if (i<0 || i>=M || j<0 || j>=N)
            return false;
        if (visited[i][j])
            return false;
        if (board[i][j] != word[idx])
            return false;
        
        visited[i][j] = 1;
        bool ans = DFS(board, i+1, j, M, N, visited, word, idx+1)
        || DFS(board, i-1, j, M, N, visited, word, idx+1)
        || DFS(board, i, j+1, M, N, visited, word, idx+1)
        || DFS(board, i, j-1, M, N, visited, word, idx+1);
        visited[i][j] = 0;
        return ans;
    }

    bool exist(vector<vector<char>>& board, string word) {
        if (board.empty())
            return false;
        int M = board.size(), N = board[0].size();
        vector<vector<int>> visited(M, vector<int>(N, 0));
        for (int i = 0; i < M; i++)
            for (int j = 0; j < N; j++)
                if (DFS(board, i, j, M, N, visited, word, 0))
                    return true;
        return false;
    }
};

```

<br>

