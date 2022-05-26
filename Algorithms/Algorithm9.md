## 目录
- [九.BFS专题](#九bfs专题)
    - [127.单词接龙](#127单词接龙)
    - [126.单词接龙2](#126单词接龙2)


### 九.BFS专题

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


* **解法一**


解题思路：DFS遍历法，此方法需要遍历所有的情况，则会导致题目超时

时间复杂度：O()

空间复杂度：O()


```cpp
class Solution {
    int ans = INT32_MAX;
public:
    bool check(string& word1, string& word2)
    {
        int diff = 0;
        for (int i = 0; i < word1.size(); ++i)
            if (word1[i] != word2[i])
                ++diff;
        return diff == 1;
    }

    void DFS(string& beginWord, string& endWord, vector<string>& wordList, int level, vector<int>& visited)
    {
        if (beginWord == endWord)
        {
            ans = std::min(ans, level);
            return;
        }

        for (int i = 0; i < wordList.size(); ++i)
        {
            if (visited[i])
                continue;
            if (!check(beginWord, wordList[i]))
                continue;
            visited[i] = 1;
            DFS(wordList[i], endWord, wordList, level+1, visited);
            visited[i] = 0;
        }
    }

    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        vector<int> visited(wordList.size(), 0);
        DFS(beginWord, endWord, wordList, 1, visited);
        return ans==INT32_MAX ? 0 : ans;
    }
};

```


* **解法二**

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


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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

解题思路：将DFS和BFS分开使用，结果超时。

时间复杂度：

空间复杂度：

```cpp
class Solution {
    vector<vector<string>> ans;
public:
    bool check(string& word1, string& word2)
    {
        int diff = 0;
        for (int i = 0; i < word1.size(); ++i)
            if (word1[i] != word2[i])
                ++diff;
        return diff == 1;
    }



    int BFS(string& beginWord, string& endWord, vector<string>& wordList) {
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
                    if (check(s, wordList[i]))
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

    void DFS(string& beginWord, string& endWord, vector<string>& wordList, int level, int height,  vector<int>& visited, vector<string>& path)
    {
        if (level == height)
        {
            if (beginWord == endWord)
            {
                path.push_back(beginWord);
                ans.push_back(path);
                path.pop_back();
            }
            return;
        }

        for (int i = 0; i < wordList.size(); ++i)
        {
            if (visited[i])
                continue;
            if (!check(beginWord, wordList[i]))
                continue;
            visited[i] = 1;
            path.push_back(beginWord);
            DFS(wordList[i], endWord, wordList, level+1, height,  visited, path);
            visited[i] = 0;****
            path.pop_back();
        }
    }

    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        int height = BFS(beginWord, endWord, wordList);
        if (0 == height)
            return {};
        vector<int> visited(wordList.size(), 0);
        vector<string> path;
        DFS(beginWord, endWord, wordList, 1, height, visited, path);
        return ans;
    }
};

```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


