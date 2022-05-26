## 目录
- [八.暴力枚举专题](#八暴力枚举专题)
    - [78.子集](#78子集)
    - [90.子集2](#90子集2)
    - [77.组合](#77组合)
    - [39.组合总合](#39组合总合)
    - [40.组合总合2](#40组合总合2)
    - [46.全排列](#46全排列)
    - [47.全排列2](#47全排列2)
    - [60.排列序列](#60排列序列)
    - [31.下一个排列](#31下一个排列)


### 八.暴力枚举专题


---------------------------
##### 78.子集 
>题目描述:给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/subsets

解题思路：子集的本质是求 组合 ，此题也实质上是 组合 的变形题，用DFS即可。需要求解出所有解。

时间复杂度：O(n* 2^n)，将nums数组的长度看成n，用二进制位的思想组合子集即可算出 时间复杂度

空间复杂度：O(N)

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


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 90.子集2
>题目描述:
给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/subsets-ii

解题思路：在 子集1 的基础上进行剪枝即可。

时间复杂度：O(N * 2^n)

空间复杂度：O(N)

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





<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 77.组合
>题目描述:给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combinations

解题思路：DFS，该题与全排列的不同之处在于组合的数字要求递增，那么也就是在DFS中需要设置起始位start，并不断往后回溯。

时间复杂度：O(2^K)

空间复杂度：O(K)

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




<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 39.组合总合
>题目描述:给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的数字可以无限制重复被选取。
说明：
所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combination-sum

解题思路：DFS法，需要设置辅助变量path记录路径，以及每次遍历的起始点start。

时间复杂度：O(N!)

空间复杂度：O(N)

```cpp
class Solution {
    vector<vector<int>> ans;
public:
    
    void DFS(vector<int>& nums, int start, int target, vector<int>& path)
    {
        if (0 == target)
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i < nums.size(); ++i)
        {
            if (target < nums[i])
                return;
            path.push_back(nums[i]);
            DFS(nums, i, target - nums[i], path);
            path.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end(), std::less<int>());
        vector<int> path;
        DFS(candidates, 0, target, path);
        return ans;
    }
};

```


<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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
    
    void DFS(vector<int>& nums, int start, int target, vector<int>& path)
    {
        if (0 == target)
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i < nums.size(); ++i)
        {
            if (target < nums[i])
                return;
            if (i>start && nums[i]==nums[i-1])
                continue;
            path.push_back(nums[i]);
            DFS(nums, i + 1, target - nums[i], path);
            path.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end(), std::less<int>());
        vector<int> path;
        DFS(candidates, 0, target, path);
        return ans;
    }
};



```

<br>





<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------
##### 60.排列序列

>题目描述：给出集合 [1,2,3,...,n]，其所有元素共有 n! 种排列。
按大小顺序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：
"123"
"132"
"213"
"231"
"312"
"321"
给定 n 和 k，返回第 k 个排列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/permutation-sequence


* **解法一**

解题思路：按照求全排列的思路进行计算，该方法超时

时间复杂度：O(N*N!)

空间复杂度：O(N)


```cpp
class Solution {
    int cnt = 0;
    string ans;
public:
    void DFS(string& s, int k, string& path, vector<int>& visited)
    {
        if (path.size() == s.size())
        {
            cnt++;
            if (cnt == k)
                ans = path;
            return;
        }

        for (int i = 0; i < s.size(); i++)
        {
            if (visited[i])
                continue;
            visited[i] = 1;
            path.push_back(s[i]);
            DFS(s, k, path, visited);
            path.pop_back();
            visited[i] = 0;
        }
    }

    string getPermutation(int n, int k) {
        if (n<=0 || k<=0)
            return string();
        string s, path;
        for (int i = 1; i <= n; i++)
            s.push_back(i+'0');
        vector<int> visited(n, 0);
        DFS(s, k, path, visited);
        return ans;
    }
};

```

* **解法二**


解题思路：在上一方法的基础上进行优化，用DFS递归的方法来进行无回溯的一次遍历，由于中间需要计算阶乘，所以时间复杂度为O(N^2)。

时间复杂度：O(N^2)

空间复杂度：O(N)


```cpp

class Solution {
    string ans;
public:
    int getFactorial(int num)
    {
        int ans = 1;
        for (int i = 1; i <= num; ++i)
            ans *= i;
        return ans;
    }

    void DFS(string& s, vector<int>& visited, string& path, int& k)
    {
        if (0 == k)
            return;
        if (path.size()==visited.size() && 1==k)
        {
            ans = path;
            k = 0;
            return;
        }
        int num = getFactorial(s.size() - path.size());
        if (k > num)
        {
            k -= num;
            return;
        }
        
        for (int i = 0; i < s.size(); ++i)
        {
            if (visited[i])
                continue;
            visited[i] = 1;
            path.push_back(s[i]);
            DFS(s, visited, path, k);
            path.pop_back();
            visited[i] = 0;
        }
    }

    string getPermutation(int n, int k) {
        string s;
        for (int i = 1; i <= n; ++i)
            s.push_back(i + '0');
        vector<int> visited(n, 0);
        string path;
        DFS(s, visited, path, k);
        return ans;
    }
};
```

<br>




<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


----------------------------
##### 31.下一个排列

>题目描述：实现获取 下一个排列 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。
如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。
必须 原地 修改，只允许使用额外常数空间。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/next-permutation

解题思路：两次遍历再加上局部反转即可；第一次遍历从rbegin出发找到第一个降序的数nums[i]，第二次遍历从rbegin出发找到第一个大于nums[i]的数，交换后从i的位置往后进行反转。具体代码为两个for if 即可搞定。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int l = nums.size()-2;
        for (; l >= 0 && nums[l] >= nums[l+1]; --l);
        if (l < 0)
        {
            std::reverse(nums.begin(), nums.end());
        }else
        {
            int r = nums.size() - 1;
            while (nums[l] >= nums[r])
                --r;
            swap(nums[l], nums[r]);
            std::reverse(nums.begin() + l + 1, nums.end());
        }
    }
};
```

<br>

<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>



