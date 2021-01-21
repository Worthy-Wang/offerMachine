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
        - [44.通配符匹配](#44通配符匹配)
        - [10.正则表达式匹配](#10正则表达式匹配)
        - [32.最长的有效括号](#32最长的有效括号)
        - [84.柱状图中最大的矩形](#84柱状图中最大的矩形)
        - [239. 滑动窗口最大值](#239-滑动窗口最大值)
        - [99.恢复二叉搜索树](#99恢复二叉搜索树)
        - [124.二叉树中的最大路径和](#124二叉树中的最大路径和)
        - [23.合并K个升序链表](#23合并k个升序链表)
        - [剑指 Offer 51. 数组中的逆序对](#剑指-offer-51-数组中的逆序对)
        - [剑指 Offer 41. 数据流中的中位数](#剑指-offer-41-数据流中的中位数)
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
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        dp[0][0] = 1;
        for (int i = 1; i <= m; ++i)
            dp[i][0] = 0;
        for (int j = 1; j <= n; ++j)
            if ('*'==p[j-1])
                dp[0][j] = 1;
            else
                break;
        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
            {
                if (p[j-1] == s[i-1])
                    dp[i][j] = dp[i-1][j-1];
                else if ('?' == p[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else if ('*' == p[j-1])
                    dp[i][j] = dp[i-1][j] || dp[i][j-1];
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
        vector<vector<int>> dp(m+1, vector<int>(n+1, false));
        dp[0][0] = true;
        for (int i = 1; i <= m; ++i)
            dp[i][0] = false;
        for (int j = 1; j <= n; ++j)
            if ('*' == p[j-1])
                dp[0][j] = dp[0][j-2];
            else
                dp[0][j] = false;
        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
            {
                if (s[i-1]==p[j-1] || '.'==p[j-1])
                {
                    dp[i][j] = dp[i-1][j-1];
                }else if ('*' == p[j-1])
                {
                    if (dp[i][j-2])
                        dp[i][j] = true;
                    else
                    {
                        if (p[j-2]==s[i-1] || '.'==p[j-2])
                            dp[i][j] = dp[i-1][j];
                    }
                }
            }
        return dp[m][n];
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



