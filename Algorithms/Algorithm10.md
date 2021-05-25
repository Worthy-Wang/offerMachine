- [十.DFS专题](#十dfs专题)
    - [139.单词拆分](#139单词拆分)
    - [140.单词拆分2](#140单词拆分2)
    - [131.分割回文串](#131分割回文串)
    - [132.分割回文串2](#132分割回文串2)
    - [79.单词搜索](#79单词搜索)
    - [130.被围绕的区域](#130被围绕的区域)
    - [36.有效的数独](#36有效的数独)
    - [37.解数独](#37解数独)
    - [51.N皇后](#51n皇后)
    - [52.N皇后2](#52n皇后2)


### 十.DFS专题


---------------------------
##### 139.单词拆分
>题目描述:给定一个非空字符串 s 和一个包含非空单词的列表 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。
说明：
拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-break

* **解法一**

解题思路：DFS回溯算法，该方法最容易想到，但是由于该题的结果并不是求解所有的结果，而只是判断是否能够拆分单词，所以动态规划才是较好的方法（DFS通常会超时）。

时间复杂度：

空间复杂度：

```cpp
class Solution {
    bool ans = false;
public:
    void DFS(string& s, int start, unordered_set<string>& hashset)
    {   
        if (ans)
            return;
        if (start == s.size())
        {
            ans = true;
            return;
        }

        for (int i = start; i < s.size(); ++i)
        {
            if (hashset.find(s.substr(start, i-start+1)) != hashset.end())
                DFS(s, i + 1, hashset);
        }
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> hashset(wordDict.begin(), wordDict.end());
        DFS(s, 0, hashset);
        return ans;
    }
};
```


* **解法二**

解题思路：这一类的问题实质上都属于背包算法，有时间可以总结这一类的问题（该题的leetcode题解中有相关解答）。
该题采用动态规划，创建动态规划数组存放哪些位置可以被拆分。

时间复杂度：O(N^2) N是s的长度，M是wordDict的长度 

空间复杂度：O(M+N)

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> hashset(wordDict.begin(), wordDict.end());
        int n = s.size();
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < n; ++i)
        {
            if (!dp[i])
                continue;
            for (int j = i + 1; j <= n; ++j)
            {
                if (hashset.find(s.substr(i, j-i)) != hashset.end())
                    dp[j] = 1;
            }
        }
        return dp[n];
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

解题思路：递归，类似于求全排列的方法

时间复杂度：

空间复杂度：

```cpp
class Solution {
    vector<string> ans;
public:
    void DFS(string& s, int start, unordered_set<string>& hashset, vector<string>& path)
    {
        if (start == s.size())
        {
            string str;
            for (const auto& e: path)
                str = str + e + " ";
            str.pop_back();
            ans.push_back(std::move(str));
            return;
        }

        for (int i = start; i < s.size(); ++i)
        {
            if (hashset.find(s.substr(start, i - start + 1)) != hashset.end())
            {
                path.push_back(s.substr(start, i - start + 1));
                DFS(s, i + 1, hashset, path);
                path.pop_back();
            }
        }
    }

    vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> hashset(wordDict.begin(), wordDict.end());
        int start = 0;
        vector<string> path;
        DFS(s, start, hashset, path);
        return ans;
    }
};
```

<br>



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
>题目描述:给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。
返回符合要求的最少分割次数。
示例:
输入: "aab"
输出: 1
解释: 进行一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-partitioning-ii

* **解法一**

解题思路：经典的动态规划题目，进行一次遍历string，先检测该子串是否会回文串，是则分割次数为0；否则在右边部分满足是回文串的情况下，查看左边部分的最小分割次数。

时间复杂度：O(N^3)

空间复杂度：O(N)


```cpp
class Solution {
public:
    bool isPalin(string& s, int l, int r)
    {
        int i = l, j = r;
        while (i < j)
            if (s[i++] != s[j--])
                return false;
        return true;
    }

    int minCut(string s) {
        int n = s.size();
        //dp[i]代表s[0]到s[i]的最少分割次数
        vector<int> dp(n);
        for (int i = 0; i < n; ++i)
            dp[i] = i; //1个字符最少分割0次，2个字符最少分割1次
        
        for (int i = 0; i < n; ++i)
        {
            if (isPalin(s, 0, i))
                dp[i] = 0;
            else
            {
                for (int mid = 0; mid < i;  ++mid)
                    if (isPalin(s, mid+1, i - mid))
                        dp[i] = std::min(dp[i], dp[mid] + 1);
            }
        }

        return dp[n-1];
    }
};

```

<br>



* **解法二**

解题思路：在上一个解法的基础上对isPalin函数进行优化，将其优化为二维的动态规划数组，相当于空间换时间

时间复杂度：O(N^2)

空间复杂度：O(N^2)

```cpp
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        //isPalin[i][j]代表s[i]到s[j]的子串是否为回文串
        vector<vector<bool>> isPalin(n, vector<bool>(n, false));
        for (int j = 0; j < n; ++j)
            for (int i = 0; i <= j; ++i)
                if (s[i]==s[j] && (j-i<=2 || isPalin[i+1][j-1]))
                    isPalin[i][j] = true;
        //dp[i]代表s[0]到s[i]的最少分割次数
        vector<int> dp(n);
        for (int i = 0; i < n; ++i)
            dp[i] = i; //1个字符最少分割0次，2个字符最少分割1次
        
        for (int i = 0; i < n; ++i)
        {
            if (isPalin[0][i])
                dp[i] = 0;
            else
            {
                for (int mid = 0; mid < i;  ++mid)
                    if (isPalin[mid+1][i])
                        dp[i] = std::min(dp[i], dp[mid] + 1);
            }
        }

        return dp[n-1];
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



---------------------------
##### 130.被围绕的区域
>题目描述:给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。
找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。
被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/surrounded-regions

解题思路：DFS法，并通过设置visited数组判断是否已经走过该点，visited数组最后可以作为置X的标准。

时间复杂度：O(M*N)

空间复杂度：O(N*M)

```cpp
class Solution {
public:
    void DFS(vector<vector<char>>& board, int i, int j, int M, int N, vector<vector<int>>& visited)
    {
        if (i<0 || i>=M || j<0 || j>=N)
            return;
        if (visited[i][j] || 'X'==board[i][j])
            return;
        
        visited[i][j] = 1;
        DFS(board, i+1, j, M, N, visited);
        DFS(board, i-1, j, M, N, visited);
        DFS(board, i, j+1, M, N, visited);
        DFS(board, i, j-1, M, N, visited);
    }

    void solve(vector<vector<char>>& board) {
        if (board.empty())
            return;
        int M = board.size(), N = board[0].size();
        vector<vector<int>> visited(M, vector<int>(N, 0));
        for (int i = 0; i < M; i++)
        {
            DFS(board, i, 0, M, N, visited);
            DFS(board, i, N-1, M, N, visited);
        }
        for (int j = 0; j < N; j++)
        {
            DFS(board, 0, j, M, N, visited);
            DFS(board, M-1, j, M, N, visited);
        }
        
        for (int i = 1; i < M-1; i++)
            for (int j = 1; j < N-1; j++)
                if ('O'==board[i][j] && !visited[i][j])
                    board[i][j] = 'X';
    }
};

```

<br>





-----------------------------------
##### 36.有效的数独
>题目描述：判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。
数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-sudoku

解题思路：设置行，类，宫三种类型的辅助二维数组，每一个二维数组有9行，每一行可以存储 1~9 10个数字，进行一次遍历即可。

时间复杂度：O(1)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        if (board.empty())
            return false;
        vector<vector<int>> rows(9, vector<int>(10, 0));
        vector<vector<int>> columns(9, vector<int>(10, 0));
        vector<vector<vector<int>>> boxes(3, vector<vector<int>>(3, vector<int>(10, 0)));
        //int rows[9][10] = {0}, cols[9][10] = {0}, boxes[3][3][10] = {0}; 或者直接这样写也行
        for (int i = 0; i < 9; i++)
        {
            for (int j = 0; j < 9; j++)
            {
                if (board[i][j] != '.')
                {
                    int num = board[i][j] - '0';
                    if (rows[i][num] || columns[j][num] || boxes[i/3][j/3][num])
                        return false;
                    rows[i][num]++;
                    columns[j][num]++;
                    boxes[i/3][j/3][num]++;
                }
            }
        }
        return true;
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

解题思路：解题思路与上一题相似，设置三个辅助数组判断数字有没有在 行，列，宫 中出现，将所有的空白存入数组依次进行填充，并记录下填充空白的个数；然后使用递归法一次填充空白即可。

时间复杂度：

空间复杂度：

```cpp
class Solution {
    int rows[9][10] = {0};
    int cols[9][10] = {0};
    int boxes[3][3][10] = {0};
    vector<pair<int,int>> spaces; //存储目前未充填的位置
    vector<vector<char>> ans;
public:
    void DFS(vector<vector<char>>& board, int idx)
    {
        if (idx == spaces.size())
        {
            ans = board;
            return;
        }
        pair<int,int> space = spaces[idx];
        int i = space.first, j = space.second;
        for (int num = 1; num <= 9; ++num)
        {
            if (rows[i][num] || cols[j][num] || boxes[i/3][j/3][num])
                continue;
            rows[i][num] = 1, cols[j][num] = 1, boxes[i/3][j/3][num] = 1;
            board[i][j] = num + '0';
            DFS(board, idx + 1);
            rows[i][num] = 0, cols[j][num] = 0, boxes[i/3][j/3][num] = 0;
            board[i][j] = '.';
        }
    }

    void solveSudoku(vector<vector<char>>& board) {
        for (int i = 0; i < 9; ++i)
        {
            for (int j = 0; j < 9; ++j)
            {
                if ('.' == board[i][j])
                {
                    spaces.push_back(make_pair(i,j));
                }
                else
                {
                    int num = board[i][j] - '0';
                    rows[i][num] = 1;
                    cols[j][num] = 1;
                    boxes[i/3][j/3][num] = 1; 
                }
            }
        }
        DFS(board, 0);
        board = ans;
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
class Solution
{
    vector<vector<string>> ans;
    unordered_map<int,int> cols, line1, line2;
public:
    void DFS(int n, int i, vector<string>& board)
    {
        if (i == n)
        {
            ans.push_back(board);
            return;
        }

        for (int j = 0; j < n; ++j)
        {
            if (cols[j] || line1[j + i] || line2[j - i])
                continue;
            board[i][j] = 'Q';
            cols[j] = 1, line1[j + i] = 1, line2[j - i] = 1;
            DFS(n, i + 1, board);
            cols[j] = 0, line1[j + i] = 0, line2[j - i] = 0;
            board[i][j] = '.';
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<string> board(n, string(n, '.'));
        DFS(n, 0, board);
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
    int cnt = 0;
    unordered_map<int,int> cols, line1, line2;
public:
    void DFS(int n, int i)
    {
        if (i == n)
        {
            ++cnt;
            return;
        }

        for (int j = 0; j < n; ++j)
        {
            if (cols[j] || line1[j-i] || line2[j+i])
                continue;
            cols[j] = 1, line1[j-i] = 1, line2[j+i] = 1;
            DFS(n, i + 1);
            cols[j] = 0, line1[j-i] = 0, line2[j+i] = 0;
        }
    }

    int totalNQueens(int n) {
        DFS(n, 0);
        return cnt;
    }
};

```

<br>



