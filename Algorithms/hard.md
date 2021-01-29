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
    - [25.K个一组翻转链表](#25k个一组翻转链表)
    - [65.有效数字](#65有效数字)
    - [28.实现strStr()](#28实现strstr)
    - [132.分割回文串2](#132分割回文串2)
    - [44.通配符匹配](#44通配符匹配)
    - [10.正则表达式匹配](#10正则表达式匹配)
    - [97.交错字符串](#97交错字符串)
    - [72.编辑距离](#72编辑距离)
    - [32.最长的有效括号](#32最长的有效括号)
    - [84.柱状图中最大的矩形](#84柱状图中最大的矩形)
    - [85.最大矩阵](#85最大矩阵)
    - [239. 滑动窗口最大值](#239-滑动窗口最大值)
    - [99.恢复二叉搜索树](#99恢复二叉搜索树)
    - [124.二叉树中的最大路径和](#124二叉树中的最大路径和)
    - [23.合并K个升序链表](#23合并k个升序链表)
    - [剑指 Offer 51. 数组中的逆序对](#剑指-offer-51-数组中的逆序对)
    - [剑指 Offer 41. 数据流中的中位数](#剑指-offer-41-数据流中的中位数)
    - [60.排列序列](#60排列序列-1)
    - [127.单词接龙](#127单词接龙)
    - [87.扰乱字符串](#87扰乱字符串)
    - [45.跳跃游戏2](#45跳跃游戏2)
    - [123.买卖股票的最佳时机3](#123买卖股票的最佳时机3)
    - [115.不同的子序列](#115不同的子序列)
    - [128.最长连续序列](#128最长连续序列)
    - [76.最小覆盖子串](#76最小覆盖子串)
    - [30.串联所有单词的子串](#30串联所有单词的子串)

### leetcode精选高频hard题目


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

解题思路：依次遍历各个点，与后面的点进行计算斜率，找出同一个斜率中拥有的最多点数即可。需要注意两点：1.斜率可能出现与x轴或者y轴相平行，所以斜率的形式要用string表示；2.可能会出现重复的点的情况，所以用dup记录重复的点的个数。

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int GCD(int a, int b)
    {
        while (b)
        {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }

    int maxPoints(vector<vector<int>>& points) {
        int ans = 0;
        for (int i = 0; i < points.size(); ++i)
        {
            unordered_map<string, int> hashmap;//<斜率,点个数>
            int curMax = 0;
            int dup = 0;
            for (int j = i+1; j < points.size(); ++j)
            {
                int x1 = points[i][0], y1 = points[i][1];
                int x2 = points[j][0], y2 = points[j][1];
                int diffX = x2 - x1, diffY = y2 - y1, gcd = GCD(diffX, diffY);
                if (0==diffX && 0==diffY)
                {
                    dup++;
                    continue;
                }
                string key = to_string(diffX/gcd) + "/" + to_string(diffY/gcd);
                hashmap[key]++;
                curMax = std::max(curMax, hashmap[key]);
            }
            ans = std::max(curMax + 1 + dup, ans); //curMax+1是因为要加上i指向的这一个点, 加上dup是因为要加上重复的点
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



-----------------------------------
##### 25.K个一组翻转链表
>题目描述：给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。
示例：
给你这个链表：1->2->3->4->5
当 k = 2 时，应当返回: 2->1->4->3->5
当 k = 3 时，应当返回: 3->2->1->4->5
你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-nodes-in-k-group

解题思路：对链表进行k个k个的遍历，先专门写一个函数用于转置链表的k个元素（需要返回新的首尾节点），该题相当于是 翻转链表 题的难度加大题。该题的关键点在于设置好指针，指向 上一个链表的尾结点 和 下一个链表的头结点。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    pair<ListNode*, ListNode*> reverseList(ListNode* head, ListNode* tail)
    {
        ListNode dummy(-1);
        dummy.next = head;
        ListNode* cur = head, *end = tail->next;
        while (cur != end)
        {
            auto t = cur->next;
            cur->next = dummy.next;
            dummy.next = cur;
            cur = t;
        }
        return make_pair(tail, head);
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        if (k<=1 || !head || !head->next)
            return head;
        ListNode dummy(-1);
        dummy.next = head;
        ListNode* pre = &dummy;
        while (1)
        {
            ListNode* cur = pre;
            for (int i = 0; i < k; i++)
            {
                cur = cur->next;    
                if (!cur)
                    return dummy.next;
            }
            ListNode* rightHead = cur->next;
            pair<ListNode*,ListNode*> newPair = reverseList(pre->next, cur);
            ListNode* newHead = newPair.first, *newTail = newPair.second;
            pre->next = newHead;
            pre = newTail;
            newTail->next = rightHead;
        }
        return dummy.next;
    }
};

```

<br>



-----------------------------
##### 65.有效数字
>题目描述：验证给定的字符串是否可以解释为十进制数字。
说明: 我们有意将问题陈述地比较模糊。在实现代码之前，你应当事先思考所有可能的情况。这里给出一份可能存在于有效十进制数字中的字符列表。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-number

解题思路：先写一个最难的   -3.12e-3    .前后有数字即可，e可存在可不存在，若e存在后面就必须接上数字。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isNumber(string s) {
        int i = 0;
        while (isspace(s[i])) i++; //跳过空格
        if ('+'==s[i] || '-'==s[i]) i++; //跳过+-号
        bool ans1 = false, ans2 = false;
        while(isdigit(s[i]))
        {
            i++;
            ans1 = true;
        }
        if ('.' == s[i]) i++; //跳过.
        while(isdigit(s[i]))
        {
            i++;
            ans2 = true;
        }
        if (!ans1 && !ans2) //如果小数点前后都没有数字，返回false
            return false;

        if ('e'==s[i] || 'E'==s[i])//如果数字后面有e的情况
        {
            i++;
            if ('+'==s[i] || '-'==s[i])
                i++;
            bool ans3 = false;
            while(isdigit(s[i]))
            {
                i++;
                ans3 = true;
            }
            if (!ans3)
                return false;
        }        

        //有效数字遍历完成，往后遍历空格
        while (i != s.size())
        {
            if (!isspace(s[i]))
                return false;
            i++;
        }

        return true;
    }
};

```

<br>



-----------------------------
##### 28.实现strStr()
>题目描述：实现 strStr() 函数。
给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。
示例 1:
输入: haystack = "hello", needle = "ll"
输出: 2
当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。
对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/implement-strstr

* **解法一**

解题思路：BF暴力法，从每一个haystack的元素开始遍历，往后看是否满足 needle字符串。该方法超时！

时间复杂度：O(N*M)

空间复杂度：O(1)


* **解法二**

解题思路：RK算法，首先对needle进行哈希求值并求出长度n，再对haystack进行一次遍历并在haystack中不断计算n长度的哈希值，若haystakc和needle的哈希值相等时，需要进行一遍对比（因为可能会出现哈希冲突的情况），如果仍然相等则说明两个字符串匹配。
RK算法的缺点就是如果哈希冲突太多的话，就会退化成BF算法，这个也很好理解。


时间复杂度：O(M+N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int sum1 = 0, sum2 = 0, len1 = haystack.size(), len2 = needle.size();
        for (auto& e: needle)
            sum2 += e;
        int start = 0, end = len2-1;
        for (int i = start; i <= end; i++)
            sum1 += haystack[i];
        while (end < len1)
        {
            if (sum1 == sum2)
            {
                bool flag = true;//预防哈希冲突
                for (int i = 0; i < len2; i++)
                    if (haystack[i+start] != needle[i])
                    {
                        flag = false;
                        break;
                    }
                if (flag)
                   return start;
            }
            end++;
            start++;
            if (end < len1)
                sum1 = sum1 + haystack[end]-haystack[start-1];
        }
        return -1;
    }
};

```

* **解法三**

解题思路：KMP算法，在haystack设置永不回退的指针i，在needle上设置动态回退的指针j，其中需要辅助数组next，每一次匹配失败时，**j = next[j]** ，也就是回退到之前已经拥有重复前缀的位置。
next数组其实也很好理解，需要用到动态规划的思想。它根据 前缀的前一部分 和 前缀的后一部分 是否相等，用来记录匹配失败时j指针需要回退的位置。因为如果 前缀的后一部分 能够匹配成功，那么 前缀的后一部分 也能够匹配成功，也就无需再一次让 j 指针从头开始，这就是next 数组的原理。

时间复杂度：O(M+N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    void getNext(string& needle, vector<int>& next)
    {
        int i = 0, j = -1;
        next[0] = -1;
        while (i < needle.size())
        {
            if (-1==j || needle[i]==needle[j])
            {
                ++i, ++j;
                next[i] = j;
            }else
                j = next[j];
        }
    }

    int strStr(string haystack, string needle) {
        int n1 = haystack.size(), n2 = needle.size();
        if (0 == n2)
            return 0;
        vector<int> next(n2 + 1);
        getNext(needle, next);
        int i = 0, j = 0;
        while (i < n1)
        {
            if (-1==j || haystack[i]==needle[j])
            {
                ++i, ++j;
                if (j == n2)
                    return i-n2;
            }else
                j = next[j];
        }
        return -1;
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
                    if (isPalin(s, mid+1, i))
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
##### 32.最长的有效括号
>题目描述:给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。
示例 1:
输入: "((()))"
输出: 3
解释: 最长有效括号子串为 "((()))"
示例 2:
输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"
示例 3:
输入: "()(())"
输出: 6
解释: 最长有效括号子串为 "()(())"

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-valid-parentheses

解题思路：先通过模拟栈的方法，记录不能够进行匹配消除的所有 '(' 的下标置位为1，然后该问题就进行了转换，也就是在只有0 和 1 的数组中求 连续0 的最大长度，可以以 1 作为界限，计算中间区域的长度。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        
        stack<int> stk;
        int n = s.size();
        for (int i = 0; i < n; ++i)
        {
            if (!stk.empty() && '('==s[stk.top()] && ')'==s[i])
                stk.pop();
            else
                stk.push(i);
        }

        int r = n;
        int ans = 0;
        while (!stk.empty())
        {
            int l = stk.top();
            stk.pop();
            ans = std::max(r-l-1, ans);
            r = l;
        }
        ans = std::max(ans, r-0);
        return ans;
    }
};

```

<br>




---------------------------
##### 84.柱状图中最大的矩形
>题目描述:给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
求在该柱状图中，能够勾勒出来的矩形的最大面积。
示例:
输入: [2,1,5,6,2,3]
输出: 10

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/largest-rectangle-in-histogram

* **解法一**

解题思路：暴力法，两层for循环，采用中心扩散法，第一次遍历中的柱子作为最低的高度向两边扩散。

时间复杂度：O(N^2)

空间复杂度：O(1)


* **解法二**

解题思路：单调栈法，在暴力法的基础上进行改进。在暴力法中是以当前柱体为最低高度向左右两边展开，需要分别找到 左边第一个小于当前柱体高度的 和 右边第一个小于当前柱体高度的。而单调栈可以帮助我们完成该任务，当 当前柱体的高 小于 栈顶的高时，栈顶出栈，这样就同时找到了左右两边的更小值。注意此处操作的节点是 弹出栈的那一个。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        if (heights.empty())
            return 0;
        int ans = 0;
        heights.push_back(0);//在数组尾部加入一个最小的数，保证单调栈中的所有数都可以进行计算
        std::stack<int> stk;//单调栈存储的是下标
        for (int i = 0; i < heights.size(); i++)
        {
            while (!stk.empty() && heights[i]<heights[stk.top()])
            {
                int mid = stk.top();
                stk.pop();
                int l = stk.empty() ? -1 : stk.top();
                int r = i;
                ans = std::max(ans, heights[mid]*(r-l-1));
            }
            stk.push(i);
        }
        return ans;
    }
};

```

<br>




---------------------------
##### 85.最大矩阵
>题目描述:给定一个仅包含 0 和 1 、大小为 rows x cols 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximal-rectangle

* **解法一**

解题思路：调用上一题求最大矩阵的解法，一行一行的计算，求解出最大矩阵

时间复杂度：O(M*N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        if (heights.empty())
            return 0;
        heights.push_back(0);
        int ans = 0;
        stack<int> stk;
        for (int i = 0; i < heights.size(); i++)
        {
            while (!stk.empty() && heights[i]<heights[stk.top()])
            {
                int mid = stk.top();
                stk.pop();
                int l = stk.empty()? -1 : stk.top();
                int r = i;
                ans = std::max(ans, (r-l-1)*heights[mid]);
            }
            stk.push(i);
        }
        heights.pop_back();
        return ans;
    }

    int maximalRectangle(vector<vector<char>>& matrix) {
        if (matrix.empty())
            return 0;
        int ans = 0;
        int M = matrix.size(), N = matrix[0].size();
        vector<vector<int>> nums(M, vector<int>(N));
        for (int i = 0; i < M; i++)
            for (int j = 0; j < N; j++)
                if ('1' == matrix[i][j])
                    nums[i][j] = 1;
                else 
                    nums[i][j] = 0;

        for (int i = 1; i < M; i++)
            for (int j = 0; j < N; j++)
                if (0 == nums[i][j])
                    continue;
                else
                    nums[i][j] += nums[i-1][j];
                    
        for (int i = 0; i < M; i++)
        {
            int res = largestRectangleArea(nums[i]);
            ans = std::max(ans, res);
        }
        return ans;
    }
};

```


<br>





---------------------------
##### 239. 滑动窗口最大值
>题目描述:给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。
返回滑动窗口中的最大值。
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sliding-window-maximum

解题思路：用一个单调deque作为辅助，存放的是单调递减的元素下标，当新的元素更大不满足单调减的特性时需要从队尾出队，当deque长度超长时需要从队头出队。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        if (nums.empty() || nums.size()<k )
            return {};
        std::deque<int> deque;//存储下标
        for (int i = 0; i < k; ++i)
        {
            while (!deque.empty() && nums[i]>=nums[deque.back()])
                deque.pop_back();
            deque.push_back(i);
        }
        vector<int> ans{nums[deque.front()]};
        for (int i = k; i < nums.size(); i++)
        {
            if (i - deque.front() >= k)
                deque.pop_front();
            while (!deque.empty() && nums[i]>=nums[deque.back()])
                deque.pop_back();
            deque.push_back(i);
            ans.push_back(nums[deque.front()]);
        }
        return ans;
    }
};


```

<br>



--------------------------- 
##### 99.恢复二叉搜索树
>题目描述:给你二叉搜索树的根节点 root ，该树中的两个节点被错误地交换。请在不改变其结构的情况下，恢复这棵树。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/recover-binary-search-tree


解题思路：在中序遍历的过程中直接用两个指针进行指向即可。遍历过程中仍然是可能会出现一个逆序对或者两个逆序对的情况。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    TreeNode* pre = nullptr;
    TreeNode* node1 = nullptr;
    TreeNode* node2 = nullptr;
public:
    void DFS(TreeNode* root)
    {
        if (!root)
            return;
        DFS(root->left);
        if (!pre)
            pre = root;
        else
        {
            if (pre->val > root->val)
            {
                if (!node1 && !node2) //交换的节点相邻
                {
                    node1 = pre;
                    node2 = root;
                }else //交换的节点不相邻，此时node1保留
                {
                    node2 = root;
                }
            }
            pre = root;
        }
        DFS(root->right);
    }

    void recoverTree(TreeNode* root) {
        DFS(root);
        swap(node1->val, node2->val);
    }
};

```


<br>



---------------------------
##### 124.二叉树中的最大路径和
>题目描述:给定一个非空二叉树，返回其最大路径和。
本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。
注意：左子树->根->右子树 也算是路径。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-maximum-path-sum

解题思路：后序遍历的方法，不断将计算出来的结果与保留的最大值进行比较

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    int ans = INT32_MIN;
public:
    int DFS(TreeNode* root)
    {
        if (!root)
            return 0;
        int l = DFS(root->left);
        int r = DFS(root->right);
        ans = std::max(ans, root->val);
        ans = std::max(ans, root->val + max(0,l) + max(0,r));
        return root->val + std::max(0, std::max(l, r));
    }

    int maxPathSum(TreeNode* root) {
        if (!root)
            return 0;
        DFS(root);
        return ans;
    }
};
```

<br>




---------------------------
##### 23.合并K个升序链表
>题目描述:给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-k-sorted-lists

* **解法一**

解题思路：用一个for循环，将链表数组中的所有链表都依次按照 两个升序链表合并 的方法进行合并。

时间复杂度：O(N + 2N + 3N +  .. + KN) ~= O(K^2 * N) N为链表的平均长度，K为链表的个数

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2)
    {
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        while (l1 && l2)
        {
            if (l1->val <= l2->val)
            {
                tail->next = l1;
                tail = l1;
                l1 = l1->next;
            }
            else 
            {
                tail->next = l2;
                tail = l2;
                l2 = l2->next;
            }
        }
        if (l1)
            tail->next = l1;
        if (l2)
            tail->next = l2;
        return dummy.next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* ans = nullptr;
        for (int i = 0; i < lists.size(); ++i)
            ans = merge(lists[i], ans);
        return ans;
    }
};

```


* **解法二**

解题思路：在解法一中进行优化，不断的两两归并链表直到只剩下最后一个链表。实质上也就是归并排序的链表处理法。

时间复杂度：O((k/2*2N + k/4*4N + ... ) * logK) ~= O(KN*logK) N为链表的平均长度， K为链表的个数

空间复杂度：O(logK) 

```cpp
 class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2)
    {
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        while (l1 && l2)
        {
            if (l1->val <= l2->val)
            {
                tail->next = l1;
                tail = l1;
                l1 = l1->next;
            }
            else 
            {
                tail->next = l2;
                tail = l2;
                l2 = l2->next;
            }
        }
        if (l1)
            tail->next = l1;
        if (l2)
            tail->next = l2;
        return dummy.next;
    }

    ListNode* mergeSort(vector<ListNode*>& lists, int l, int r)
    {
        if (l == r)
            return lists[l];
        else if (l > r)
            return nullptr;
        else
        {
            int mid = (l + r) >> 1;
            ListNode* left = mergeSort(lists, l, mid);
            ListNode* right = mergeSort(lists, mid+1, r);
            return merge(left, right);
        }
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty())
            return nullptr;
        int l = 0, r = lists.size()-1;
        return mergeSort(lists, l, r);
    }
};

```


<br>



---------------------------
##### 剑指 Offer 51. 数组中的逆序对
>题目描述:在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof

解题思路：归并排序，在进行归并的过程中顺势可以计算逆序对的数量。

时间复杂度：O(NlogN)

空间复杂度：O(logN)

```cpp
class Solution {
    int ans = 0;
public:
    void merge(vector<int>& nums, int l, int mid, int r, vector<int>& temp)
    {
        int i = l, j = mid+1, k = l;
        while (i<=mid && j<=r)
        {
            if (nums[i] <= nums[j])
                temp[k++] = nums[i++];
            else
            {
                //逆序对出现
                ans += mid - i + 1;
                temp[k++] = nums[j++];
            }
        }
        while (i <= mid)
            temp[k++] = nums[i++];
        while (j <= r)
            temp[k++] = nums[j++];
        for (int k = l; k <= r; ++k)
            nums[k] = temp[k];
    }

    void mergeSort(vector<int>& nums, int l, int r, vector<int>& temp)
    {
        if (l < r)
        {
            int mid = (l + r) >> 1;
            mergeSort(nums, l, mid, temp);
            mergeSort(nums, mid+1, r, temp);
            merge(nums, l, mid, r, temp);
        }
    }

    int reversePairs(vector<int>& nums) {
        if (nums.empty())
            return 0;
        vector<int> temp(nums.size());
        mergeSort(nums, 0, nums.size()-1, temp);
        return ans;
    }
};

```

<br>


---------------------------
##### 剑指 Offer 41. 数据流中的中位数
>题目描述:如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
例如，
[2,3,4] 的中位数是 3
[2,3] 的中位数是 (2 + 3) / 2 = 2.5
设计一个支持以下两种操作的数据结构：
void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof

解题思路：设置两个堆，左边为最大堆，右边为最小堆，且保证右边的堆始终比左边的堆大；这也就是说，如果要加入一个数，得先进左边堆，再进右边的堆。

时间复杂度：addNum:O(logN) findMedian:O(1)

空间复杂度：O(N)

```cpp
class MedianFinder {
    std::priority_queue<int, vector<int>, std::less<int>> lheap;//左边的大顶堆
    std::priority_queue<int, vector<int>, std::greater<int>> rheap;//右边的小顶堆
public:
    /** initialize your data structure here. */
    MedianFinder() {
    }
    
    void addNum(int num) {
        lheap.push(num);
        rheap.push(lheap.top());
        lheap.pop();
        if (lheap.size() < rheap.size())
        {
            lheap.push(rheap.top());
            rheap.pop();
        }
    }
    
    double findMedian() {
        int n = lheap.size() + rheap.size();
        if (n & 0x1)
            return lheap.top();
        else
            return (double)(lheap.top()+rheap.top())/2;
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

解题思路：该题使用递归解决。

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

<br>





---------------------------
##### 45.跳跃游戏2
>题目描述:给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
你的目标是使用最少的跳跃次数到达数组的最后一个位置。
假设你总是可以到达数组的最后一个位置。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jump-game-ii

解题思路：仍然是贪心法，在上一题的基础上设置每一次最长的跳跃极限，当达到该跳跃极限时，cnt++，并设置下一个跳跃的极限。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() <= 1)
            return 0;
        int end = nums[0], nextEnd = end;
        int cnt = 0;
        for (int i = 0; i < nums.size()-1; ++i)
        {
            nextEnd = std::max(nextEnd, i + nums[i]);
            if (i == end)
            {
                end = nextEnd;
                ++cnt;
            }
        }
        return cnt + 1;
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
##### 115.不同的子序列
>题目描述:给定一个字符串 s 和一个字符串 t ，计算在 s 的子序列中 t 出现的个数。
字符串的一个 子序列 是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）
题目数据保证答案符合 32 位带符号整数范围。
s 和 t 由英文字母组成

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/distinct-subsequences

解题思路：创建动态规划数组，将字符串t的子串不断在s串中进行匹配，最后生成动态规划数组。
    a  b
  1 0  0
a 1 1  0
a 1 
b 1
b 1

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


--------------------------
##### 128.最长连续序列
>题目描述：给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
要求：你可以设计并实现时间复杂度为 O(n) 的解决方案吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-consecutive-sequence


解题思路：并查集，下标代表该数字，存放的元素代表能够到达的下一个数字之前

时间复杂度：O(N)

空间复杂度：O(N)


```cpp
class Solution {
    unordered_map<int,int> union_find;//<数字，数字能够到达的下一个开区间>
public:
    int find(int i)
    {
        if (union_find.count(i))
        {
            union_find[i] = find(union_find[i]);
            return union_find[i];
        }else
            return i;
    }

    int longestConsecutive(vector<int>& nums) {
        for (auto& i: nums)
            union_find[i] = i+1;
        int ans = 0;
        for (auto& i: nums)
        {
            union_find[i] = find(union_find[i]);
            ans = std::max(ans, union_find[i] - i);
        }
        return ans;
    }
};

```


<br>





---------------------------
##### 76.最小覆盖子串
>题目描述:给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。
注意：如果 s 中存在这样的子串，我们保证它是唯一的答案。
s 和 t 由英文字母组成
进阶：你能设计一个在 o(n) 时间内解决此问题的算法吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-window-substring

解题思路： 先想到暴力法，两次 for循环，比较后找到最小的子串，需要用hashmap来存储t串中的元素。
再想到用滑动窗口的优化法，设置双指针i j ，当滑动窗口中包含t串中的所有元素时，i++； 没有包含t串中的所有元素时，j++。

时间复杂度：O(N) N为s串的长度

空间复杂度：O(M) M为t串的长度

```cpp
class Solution {
public:
    bool check(unordered_map<char,int>& sMap, unordered_map<char,int>& tMap)
    {
        for (auto& e: tMap)
            if (e.second > sMap[e.first])
                return false;
        return true;
    }

    string minWindow(string s, string t)
    {
        unordered_map<char, int> sMap, tMap; //由于s和t中都可能有重复的字符，所以需要用哈希表表示
        for (auto &ch : t)
            tMap[ch]++;
        int i = 0, j = 0;
        int minLen = INT32_MAX, beg = 0, end = -1;
        while (j < s.size())
        {
            sMap[s[j]]++;
            if (check(sMap, tMap)) //滑动窗口已经覆盖t
            {
                while (check(sMap, tMap))
                {
                    if (minLen > (j - i + 1))
                    {
                        minLen = j - i + 1;
                        beg = i;
                        end = j;
                    }
                    sMap[s[i]]--;
                    ++i;
                }
            }
            //滑动窗口未覆盖t
            ++j;
        }
        return s.substr(beg, end - beg + 1);
    }
};

```

<br>




---------------------------
##### 30.串联所有单词的子串
>题目描述:给定一个字符串 s 和一些**长度相同**的单词 words。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。
注意子串要与 words 中的单词完全匹配，中间不能有其他字符，但不需要考虑 words 中单词串联的顺序。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words

解题思路：暴力法，将words看做一个字符串进行比较即可

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
public:
    bool check(string s, unordered_map<string,int>& wordMap, int n, int m)
    {
        unordered_map<string,int> hashmap;
        for (int i = 0; i < s.size(); i+=m)
            hashmap[s.substr(i, m)]++;
        return hashmap==wordMap;
    }

    vector<int> findSubstring(string s, vector<string>& words) {
        unordered_map<string, int> wordMap;
        for (auto& e: words)
            wordMap[e]++;
        int n = words.size(), m = words[0].size(), wordsLen = m*n;
        vector<int> ans;

        for (int i = 0; i < s.size()-wordsLen +1; i++)
        {
            string s2 = s.substr(i, wordsLen);
            if (check(s2, wordMap, n, m))
                ans.push_back(i);
        }
        return ans;
    }
};

```

<br>



