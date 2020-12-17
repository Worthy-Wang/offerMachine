- [七.暴力枚举专题](#七暴力枚举专题)
        - [78.子集](#78子集)
        - [90.子集2](#90子集2)
        - [46.全排列](#46全排列)
        - [47.全排列2](#47全排列2)
        - [77.组合](#77组合)
        - [17.电话号码的字母组合](#17电话号码的字母组合)


# 七.暴力枚举专题

---------------------------
##### 78.子集 
>题目描述:给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/subsets

解题思路：子集的本质是求 组合 ，此题也实质上是 组合 的变形题，用DFS即可。需要求解出所有解。

时间复杂度：O()

空间复杂度：O()

```cpp
class Solution {
    vector<vector<int>> ans;
public:
    void DFS(vector<int>& nums, int start, vector<int>& path)
    {
        ans.push_back(path);
        for (int i = start; i < nums.size(); i++)
        {
            path.push_back(nums[i]);
            DFS(nums, i + 1, path);
            path.pop_back();
        }
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        if (nums.empty())
            return {};
        vector<int> path;
        DFS(nums, 0, path);
        return ans;
    }
};


```

<br>


---------------------------
##### 90.子集2
>题目描述:
给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/subsets-ii

解题思路：在 子集1 的基础上进行剪枝即可。

时间复杂度：

空间复杂度：

```cpp
class Solution {
    vector<vector<int>> ans;
public:
    void DFS(vector<int>& nums, int start, vector<int>& path)
    {
        ans.push_back(path);
        for (int i = start; i < nums.size(); i++)
        {
            if (start<i && nums[i]==nums[i-1])
                continue;
            path.push_back(nums[i]);
            DFS(nums, i + 1, path);
            path.pop_back();
        }
    }

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        if (nums.empty())
            return {};
        sort(nums.begin(), nums.end(), std::less<int>());
        vector<int> path;
        DFS(nums, 0, path);
        return ans;
    }
};

```

<br>


---------------------------
##### 46.全排列
>题目描述:给定一个 没有重复 数字的序列，返回其所有可能的全排列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/permutations

解题思路：DFS法，设置path变量用来存放走过的路径，visited用来保存变量是否已经访问过。

时间复杂度：O(N!)

空间复杂度：O(N)

```cpp
class Solution {
   vector<vector<int>> ans;
public:
    void DFS(vector<int>& nums, vector<int>& path, vector<int>& visited)
    {
        if (path.size() == nums.size())
        {
            ans.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++)
        {
            if (1 == visited[i])
                continue;
            visited[i] = 1;
            path.push_back(nums[i]);
            DFS(nums, path, visited);
            path.pop_back();
            visited[i] = 0;
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        if (nums.empty())
            return {};
        vector<int> path, visited(nums.size(), 0);
        DFS(nums, path, visited);
        return ans;
    }
};

```

<br>


---------------------------
##### 47.全排列2
>题目描述:给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/permutations-ii

解题思路：相比于上一题全排列，只需要先进行一次排列，并在过程中剪枝即可。

时间复杂度：O(NlogN + N!)

空间复杂度：O(N)

```cpp
class Solution {
    vector<vector<int>> ans;
public:
    void DFS(vector<int>& nums, vector<int>& path, vector<int>& visited)
    {
        if (path.size() == nums.size())
        {
            ans.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++)
        {
            if (1 == visited[i])
                continue;
            if (0<i && nums[i]==nums[i-1] && !visited[i-1])//剪枝
                continue;
            visited[i] = 1;
            path.push_back(nums[i]);
            DFS(nums, path, visited);
            path.pop_back();
            visited[i] = 0;
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        if (nums.empty())
            return {};
        sort(nums.begin(), nums.end(), std::less<int>());
        vector<int> path, visited(nums.size(), 0);
        DFS(nums, path, visited);
        return ans;
    }
};

```

<br>

---------------------------
##### 77.组合
>题目描述:给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combinations

解题思路：DFS，该题与全排列的不同之处在于组合的数字要求递增，那么也就是在DFS中需要设置起始位start，并不断往后回溯。

时间复杂度：O()

空间复杂度：O()

```cpp
class Solution {
   vector<vector<int>> ans;
public:
    void DFS(int n, int k, int start, vector<int>& path)
    {
        if (path.size() == k)
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i <= n; i++ )
        {
            path.push_back(i);
            DFS(n, k, i+1, path);
            path.pop_back();
        }
    }


    vector<vector<int>> combine(int n, int k) {
        if (n<=0 || k<=0)
            return {};
        vector<int> path;
        DFS(n, k, 1, path);
        return ans;
    }
};

```

<br>


---------------------------
##### 17.电话号码的字母组合
>题目描述:给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。此题需要看原链接。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number

解题思路：回溯法

时间复杂度：

空间复杂度：

```cpp
class Solution {
    vector<string> ans;
    unordered_map<char, string> hashmap{{'2',"abc"}, {'3',"def"},{'4',"ghi"},{'5',"jkl"},{'6',"mno"},{'7',"pqrs"},{'8',"tuv"}, {'9', "wxyz"}};
public:
    void DFS(string& digits, int start, string& path)
    {
        if (path.size() == digits.size())
        {
            ans.push_back(path);
            return;
        }

        string s = hashmap[digits[start]];
        for (int i = 0; i < s.size(); i++)
        {
            path.push_back(s[i]);
            DFS(digits, start+1, path);
            path.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        if (digits.empty())
            return {};
        string path;
        DFS(digits, 0, path);
        return ans;
    }
};

```

<br>



