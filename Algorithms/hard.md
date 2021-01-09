- [leetcode精选高频hard题目](#leetcode精选高频hard题目)
        - [4.寻找两个正序数组的中位数](#4寻找两个正序数组的中位数)
        - [60.排列序列](#60排列序列)
        - [37.解数独](#37解数独)
        - [51.N皇后](#51n皇后)
        - [52.N皇后2](#52n皇后2)
        - [149.直线上最多的点数](#149直线上最多的点数)
        - [42.接雨水](#42接雨水)
        - [135.分发糖果](#135分发糖果)
        - [41.缺失的第一个正数](#41缺失的第一个正数)
        - [](#)
        - [](#-1)

# leetcode精选高频hard题目


--------------------------
##### 4.寻找两个正序数组的中位数
>题目描述：给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。
要求：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/median-of-two-sorted-arrays

解题思路：该题用双指针法求解，不断从双指针中较小的数往后推进，并且中位数k不断二分减小，这样可以让双指针快速往后推进，达到二分效果。

时间复杂度：O(log(M+N))

空间复杂度：O(1)


```cpp
class Solution {
public:
    double findKth(vector<int>& nums1, vector<int>& nums2, int k)
    {
        int i = 0, j = 0;
        int len1 = nums1.size(), len2 = nums2.size();
        while (1)
        {
            if (i == len1)
                return nums2[j + k - 1];
            if (j == len2)
                return nums1[i + k - 1];
            if (1 == k)
                return std::min(nums1[i], nums2[j]);

            int newi = std::min(i+k/2-1, len1-1);
            int newj = std::min(j+k/2-1, len2-1);
            if (nums1[newi] < nums2[newj])
            {
                k -= newi + 1 - i;
                i = newi + 1;
            }
            else
            {
                k -= newj + 1 - j;
                j = newj + 1;
            }
        }
        return 0;
    }


    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int len = nums1.size() + nums2.size();
        if (len & 0x1)//奇数
            return findKth(nums1, nums2, len/2+1);
        else 
            return (findKth(nums1, nums2, len/2) + findKth(nums1, nums2, len/2+1))/2;        
    }
};

```

<br>




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
public:
    int getFactorial(int n)
    {
        int ans = 1;
        for (int i = 1; i <= n; i++)
            ans *= i;
        return ans;
    }

    void DFS(string& s, int k, string& ans, unordered_set<int>& visited)
    {
        for (int i = 0; i < s.size(); i++)
        {
            if (visited.count(s[i]))
                continue;
            int factorial = getFactorial(s.size()-visited.size()-1);
            if (factorial < k)
            {
                k -= factorial;
                continue;
            }
            
            ans.push_back(s[i]);
            visited.insert(s[i]);
            DFS(s, k, ans, visited);
            return;
        }
    }

    string getPermutation(int n, int k) {
        string s, ans;
        unordered_set<int> visited;
        for (int i = 1; i <= n; i++)
            s.push_back(i + '0');
        DFS(s, k, ans, visited);
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

解题思路：解题思路与上一题相似，设置三个辅助数组判断数字有没有在 行，列，宫 中出现，将所有的空白存入数组依次进行填充，并记录下填充空白的个数；然后使用递归法一次填充空白即可。

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
    unordered_map<int,int> rows,  columns, line1, line2;
public:
    void DFS(vector<string>& board, int n, int i)
    {
        if (i == n)
        {
            ans.push_back(board);
            return;            
        }

        for (int j = 0; j < n; ++j)
        {
            if (rows[i] || columns[j] || line1[i+j] || line2[i-j])
                continue;
            rows[i] = 1, columns[j] = 1, line1[i+j] = 1, line2[i-j] = 1;
            board[i][j] = 'Q';
            DFS(board, n, i+1);
            board[i][j] = '.';
            rows[i] = 0, columns[j] = 0, line1[i+j] = 0, line2[i-j] = 0;
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<string> board(n, string(n, '.'));
        DFS(board, n, 0);
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
    unordered_map<int,int> rows, columns, line1, line2;
public:
    void DFS(int i, int n)
    {
        if (i == n)
        {
            ans++;
            return;
        }

        for (int j = 0; j < n; ++j)
        {
            if (rows[i] || columns[j] || line1[i+j] || line2[i-j])
                continue;
            rows[i] = 1, columns[j] = 1, line1[i+j] = 1, line2[i-j] = 1;
            DFS(i+1, n);
            rows[i] = 0, columns[j] = 0, line1[i+j] = 0, line2[i-j] = 0;
        }
    }

    int totalNQueens(int n) {
        if (n<=0)
            return 0;
        DFS(0, n);
        return ans;
    }
};

```

<br>





---------------------------
##### 149.直线上最多的点数
>题目描述:给定一个二维平面，平面上有 n 个点，求最多有多少个点在同一条直线上。
示例 1:
输入: [[1,1],[2,2],[3,3]]
输出: 3
解释:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
示例 2:
输入: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
输出: 4
解释:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/max-points-on-a-line

解题思路：

时间复杂度：

空间复杂度：

```cpp
class Solution {
public:
    int gcd(int a, int b)
    {
            while (b != 0) {
            int temp = a % b;
            a = b;
            b = temp;
        }
        return a;
    }

    int maxPoints(vector<vector<int>>& points) {
        if (points.size() < 3)
            return points.size();
        int ans = 0;
        for (int i = 0; i < points.size(); ++i)
        {
            int dup = 0;
            int curMax = 0;
            unordered_map<string, int> hashmap;
            for (int j = i + 1; j < points.size(); ++j)
            {
                int diffX = points[j][0] - points[i][0];
                int diffY = points[j][1] - points[i][1];
                if (0==diffX && 0==diffY)
                {
                    ++dup;
                    continue;
                }
                int GCD = gcd(diffX, diffY);
                string key = to_string(diffX/GCD) + " " + to_string(diffY/GCD);
                hashmap[key]++;
                curMax = std::max(curMax, hashmap[key]);
            }
            ans = std::max(ans, curMax+1+dup);
        }
        return ans;
    }
};

```

<br>



--------------------------
##### 42.接雨水

>题目描述：给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/trapping-rain-water

* **解法一**

解题思路：暴力法，也就是一列一列的求出每一个柱子在纵向上可以存储的雨水，要能够储水也就是要形成凹槽，求出左边最大值与右边最大值，再用两个最大值中的较小值减去该柱子的高度也就可以得到纵向储水量。该暴力法会超时。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.empty())
            return 0;
        int ans = 0;
        for(int i = 1; i < height.size() - 1; i++)
        {
            int lMax = height[i], rMax = height[i];
            for(int j = i-1; j >= 0; j--)
                lMax = std::max(lMax, height[j]);
            for (int j = i + 1; j < height.size(); j++)
                rMax = std::max(rMax, height[j]);
            ans = ans + std::min(lMax, rMax) - height[i];
        }
        return ans;
    }
};

```

* **解法二**

解题思路：根据暴力法的演化得到，用动态规划数组存放该柱子左边最大值和右边最大值。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.empty())
            return 0;
        int n = height.size();
        vector<int> l(n), r(n);
        l[0] = height[0], r[n-1] = height[n-1];
        for (int i = 1; i < n; i++)
            l[i] = std::max(l[i-1], height[i]);
        for (int i = n-2; i >= 0; i--)
            r[i] = std::max(r[i+1], height[i]);
        int ans = 0;
        for(int i = 1; i < n-1; i++)
            ans = ans + std::min(l[i],r[i]) - height[i];
        return ans;
    }
};

```

* **解法三**

解题思路：双指针法，分别放置左右指针位于数组头尾部，可以让两个指针向中间靠拢，并且让左右两指针的较小值作为蓄水处（另外一个较大值就可以作为蓄水的屏障），设置左最大值和右最大值并不断更新。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.empty())
            return 0;
        int n = height.size();
        int ans = 0;
        int lMax = height[0], rMax = height[n-1];
        int l = 0, r = n-1;
        while (l < r)
        {
            if (height[l] <= height[r])
            {
                lMax = std::max(lMax, height[l]);
                ans = ans + lMax - height[l]; 
                l++;
            }
            else
            {
                rMax = std::max(rMax, height[r]);
                ans = ans + rMax - height[r];
                r--;
            }
        }
        return ans;
    }
};

```



<br>




--------------------------------
##### 135.分发糖果
>题目描述：老师想给孩子们分发糖果，有 N 个孩子站成了一条直线，老师会根据每个孩子的表现，预先给他们评分。
你需要按照以下要求，帮助老师给这些孩子分发糖果：
每个孩子至少分配到 1 个糖果。
相邻的孩子中，评分高的孩子必须获得更多的糖果。
那么这样下来，老师**至少**需要准备多少颗糖果呢？
示例 1:
输入: [1,0,2]
输出: 5
解释: 你可以分别给这三个孩子分发 2、1、2 颗糖果。
示例 2:
输入: [1,2,2]
输出: 4
解释: 你可以分别给这三个孩子分发 1、2、1 颗糖果。第三个孩子只得到 1 颗糖果，这已满足上述两个条件。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/candy


解题思路：设置辅助数组存放每个孩子的糖果数，初始值均为1（满足第一个条件），在分别从左往右，从右往左遍历增加糖果数（满足第二个条件）。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        vector<int> candy(n, 1);
        for (int i = 1; i < n; i++)
            if (ratings[i]>ratings[i-1] &&  candy[i] <= candy[i-1])
                    candy[i] = candy[i-1]+1;
        for (int i = n-2; i >= 0; i--)
            if (ratings[i] > ratings[i+1] && candy[i] <= candy[i+1])
                    candy[i] = candy[i+1]+1;
        int sum = 0;
        for (auto& i: candy)
            sum += i;
        return sum;
    }
};

```



<br>




---------------------------
##### 41.缺失的第一个正数
>题目描述:给你一个未排序的整数数组，请你找出其中没有出现的最小的正整数。
提示：
你的算法的时间复杂度应为O(n)，并且只能使用常数级别的额外空间。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/first-missing-positive

解题思路：原地哈希法，也就是将原数组作为哈希表，不断swap保证 下标 i == nums[i] 即可， 不过有下面三种情况时可以直接跳过：
1.下标i大于数组长度n  2. nums[i]为负数 3.nums[i]与nums[nums[i]]相同。
另外，由于下标0白占了一个空位，我们需要人为的添加一个0让nums的长度增加一位。
 
时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        nums.insert(nums.begin(), 0);

        for (int i = 1; i <= n; i++)
        {
            while (1<=nums[i] && nums[i]<=n && nums[i]!=i)
            {
                if (nums[i] == nums[nums[i]])
                    break;
                swap(nums[i], nums[nums[i]]);
            }
        }
        for (int i = 1; i <= n; i++)
            if (nums[i] != i)
                return i;
        return n + 1;
    }
};
```

<br>






















--------------------------
##### 
>题目描述：


解题思路：

时间复杂度：O()

空间复杂度：O()


```cpp


```

<br>




--------------------------
##### 
>题目描述：


解题思路：

时间复杂度：O()

空间复杂度：O()


```cpp


```

<br>



