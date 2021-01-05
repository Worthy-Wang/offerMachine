- [九.BFS专题](#九bfs专题)
        - [127.单词接龙](#127单词接龙)
        - [126.单词接龙2](#126单词接龙2)
        - [130.被围绕的区域](#130被围绕的区域)


# 九.BFS专题

---------------------------
##### 127.单词接龙
>题目描述:给定两个单词（beginWord 和 endWord）和一个字典，找到从 beginWord 到 endWord 的最短转换序列的长度。转换需遵循如下规则：
每次转换只能改变一个字母。
转换过程中的中间单词必须是字典中的单词。
说明:
如果不存在这样的转换序列，返回 0。
所有单词具有相同的长度。
所有单词只由小写字母组成。
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-ladder

解题思路：BFS广度优先遍历，将问题转化为找广度优先遍历树的最小匹配的深度，需要设置visited记录已访问的单词，设置辅助队列执行BFS。

时间复杂度：

空间复杂度：

```cpp
class Solution {
public:
    bool compare(string& s1, string& s2)
    {
        int diff = 0;
        for (int i = 0; i < s1.size(); i++)
            if (s1[i] != s2[i])
                diff++;
        return diff==1;
    }

    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        //BFS
        int height = 1;
        vector<int> visited(wordList.size(), 0);
        std::queue<string> que;
        que.push(beginWord);
        while (!que.empty())
        {
            std::queue<string> tQue;
            while (!que.empty())
            {
                string s = que.front();
                que.pop();
                for (int i = 0; i < wordList.size(); i++)
                {
                    if (visited[i])
                        continue;
                    if (compare(s, wordList[i]))
                    {
                        visited[i] = 1;
                        tQue.push(wordList[i]);
                        if (wordList[i] == endWord)
                            return height+1;
                    }
                }
            }
            swap(tQue, que);
            height++;
        }
        return 0;

    }
};

```

<br>


---------------------------
##### 126.单词接龙2
>题目描述:给定两个单词（beginWord 和 endWord）和一个字典 wordList，找出所有从 beginWord 到 endWord 的最短转换序列。转换需遵循如下规则：
每次转换只能改变一个字母。
转换后得到的单词必须是字典中的单词。
说明:
如果不存在这样的转换序列，返回一个空列表。
所有单词具有相同的长度。
所有单词只由小写字母组成。
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-ladder-ii

解题思路：

时间复杂度：

空间复杂度：

```cpp


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


