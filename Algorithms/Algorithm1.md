- [一.数组专题](#一数组专题)
        - [26.删除排序数组中的重复项](#26删除排序数组中的重复项)
        - [80.删除排序数组中的重复项2](#80删除排序数组中的重复项2)
        - [剑指 Offer 39. 数组中出现次数超过一半的数字](#剑指-offer-39-数组中出现次数超过一半的数字)
        - [剑指 Offer 57. 和为s的两个数字](#剑指-offer-57-和为s的两个数字)
        - [剑指 Offer 57 - II. 和为s的连续正数序列](#剑指-offer-57---ii-和为s的连续正数序列)
        - [1.两数之和](#1两数之和)
        - [15.三数之和](#15三数之和)
        - [16.最接近的三数之和](#16最接近的三数之和)
        - [18.四数之和](#18四数之和)
        - [27.移除元素](#27移除元素)
        - [60.排列序列](#60排列序列)
        - [36.有效的数独](#36有效的数独)
        - [42.接雨水](#42接雨水)
        - [48.旋转图像](#48旋转图像)
        - [66.加一](#66加一)
        - [70.爬楼梯](#70爬楼梯)
        - [89.格雷编码](#89格雷编码)
        - [73.矩阵置零](#73矩阵置零)
        - [134.加油站](#134加油站)
        - [135.分发糖果](#135分发糖果)
        - [136.只出现一次的数字](#136只出现一次的数字)
        - [137.只出现一次的数字2](#137只出现一次的数字2)
        - [260.只出现一次的数字3](#260只出现一次的数字3)
        - [剑指 Offer 03. 数组中重复的数字](#剑指-offer-03-数组中重复的数字)
        - [41.缺失的第一个正数](#41缺失的第一个正数)


# 一.数组专题


---------------------------
##### 26.删除排序数组中的重复项
>题目描述：给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array


* **解法一**

解题思路：双指针（快慢指针），如果快指针指向的值等于慢指针指向的值，那么快指针后移，否则给慢指针赋值后，快慢指针都后移。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() <= 0)
            return 0;
        if (nums.size() == 1)
            return 1;

        int s = 0, f = 1;
        for(; f < nums.size(); f++)
        {
            if (nums[s] == nums[f])
                continue;
            else
                nums[++s] = nums[f];
        }
        return s + 1;
    }
};
```

* **解法二**
  
解题思路：直接使用STL的算法库

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() <= 0)
            return 0;
        return std::distance(nums.begin(), unique(nums.begin(), nums.end()));
    }
};

```


<br>


----------------------
##### 80.删除排序数组中的重复项2
>题目描述：给定一个增序排列数组 nums ，你需要在 原地 删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii

解题思路：该题用双指针法解决，分为这三种情况：快指针和慢指针值不同，快慢指针值相同并且慢指针前后值相同，快慢指针值不同且慢指针前后值不同。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() < 3)
            return nums.size();
        int s = 1, f = 2;
        for(; f < nums.size(); f++)
        {
            if (nums[s] != nums[f])
                nums[++s] = nums[f];
            else
            {
                if (nums[s] == nums[s-1])
                    continue;
                else
                    nums[++s] = nums[f];
            }
        }
        return s + 1;
    }
};

```



<br>



---------------------------
##### 剑指 Offer 39. 数组中出现次数超过一半的数字
>题目描述:数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof

解题思路：投票法，只需要进行一次遍历即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        if (nums.empty())
            return -1;
        pair<int,int> pairs; //数字，得票
        pairs.first = nums[0], pairs.second = 1;
        for (int i = 1; i < nums.size(); i++)
        {
            if (nums[i] == pairs.first)
                pairs.second++;
            else
            {
                pairs.second--;
                if (0 == pairs.second)
                {
                    pairs.first = nums[i];
                    pairs.second = 1;
                }
            }
        }
        int cnt = 0;
        for (auto& e: nums)
            if (e == pairs.first)
                cnt++;
        return cnt>=(double)nums.size()/2 ? pairs.first : -1;
    }
};

```

<br>



---------------------------
##### 剑指 Offer 57. 和为s的两个数字
>题目描述:输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof

解题思路：双指针

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        
        int i = 0, j = nums.size()-1;
        while (i != j)
        {
            int sum = nums[i] + nums[j];
            if (sum == target)
                return vector<int>{nums[i], nums[j]};
            else if (sum < target)
                i++;
            else 
                j--;
        }
        return {};
    }
};
```

<br>




---------------------------
##### 剑指 Offer 57 - II. 和为s的连续正数序列
>题目描述:输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。
序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof

解题思路：双指针

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        if (target <= 2)
            return {};
        vector<vector<int>> ans;
        int i = 1, j = 2;
        while  (j < target)
        {
            double sum = (double)(i + j) * (j - i + 1) / 2;
            if (sum == target)
            {
                vector<int> temp;
                for (int k = i; k <= j; ++k)
                    temp.push_back(k);
                ans.push_back(std::move(temp));
                ++i, ++j;
            }else if (sum < target)
            {
                ++j;
            }    
            else
                ++i;
        }
        return ans;
    }
};
```

<br>


------------------------------
##### 1.两数之和

>题目描述：给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum

* **错误解法**

解题思路：先排序，再用双指针从首尾分别出发。此题思路行不通，因为需要返回的是数组的下标而不是数组本身！

时间复杂度：O(NlogN + N)

空间复杂度：O(1)



* **正确解法**

解题思路：将哈希表与遍历操作同时进行，相当于是一种空间换时间的策略，需要注意的地方是遍历与哈希表的创建必须同时进行，因为数组中可能会有重复元素让哈希表无法存放。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        if (nums.empty())
            return {};
        unordered_map<int,int> hashmap; //<元素，下标>
        for (int i = 0; i < nums.size(); ++i)
        {
            if (hashmap.count(target - nums[i]))
                return vector<int>{i, hashmap[target-nums[i]]};
            hashmap[nums[i]] = i;
        }
        return {};
    }
};
```

<br>

-----------------------------
##### 15.三数之和

>题目描述：给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
注意：答案中不可以包含重复的三元组。

来源：力扣（LeetCode）      
链接：https://leetcode-cn.com/problems/3sum

解题思路：先排序，再使用三指针法，也就是固定第一个指针，然后后面的两个指针用两数之和的双指针法解决。需要注意的点在于必须在执行过程中进行去重操作，不然时间复杂度会非常大！

时间复杂度：O(NlogN+N^2) ≈ O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if (nums.empty() || nums.size() < 3)
            return {};
        sort(nums.begin(), nums.end(), std::less<int>());
        vector<vector<int>> ans;
        for (int k = 0; k < nums.size()-2; ++k)
        {
            if (nums[k] > 0)
                return ans;
            if (k>0 && nums[k-1]==nums[k])
                continue;

            int i = k + 1, j = nums.size()-1;
            while (i < j)
            {
                int sum = nums[k] + nums[i] + nums[j];
                if (sum == 0)
                {
                    ans.push_back(vector<int>{nums[k], nums[i], nums[j]});
                    ++i, --j;
                    while (i<j && nums[i-1]==nums[i]) ++i;
                    while (i<j && nums[j+1]==nums[j]) --j;
                }
                else if (sum < 0)
                {
                    ++i;
                    while (i<j && nums[i-1]==nums[i]) ++i;
                }else
                {
                    --j;
                    while (i<j && nums[j+1]==nums[j]) --j;
                }
            }
        }
        return ans;
    }
};
```

<br>

---------------------------------
##### 16.最接近的三数之和

>题目描述：给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/3sum-closest


解题思路：和上一题的方法相同，唯一需要注意的是返回这三个数的和！

时间复杂度：O(NlogN+N^2) ≈ O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if (nums.size() < 3)
            return -1;
        sort(nums.begin(), nums.end());
        int ans = nums[0] + nums[1] + nums[2];
        for (int k = 0; k < nums.size()-2; k++)
        {
            int i = k + 1, j = nums.size()-1;
            while (i < j)
            {
                int sum = nums[k] + nums[i] + nums[j];
                if (sum == target)
                    return sum;
                else if (sum < target)
                {
                    if (abs(target-sum) < abs(target-ans))
                        ans = sum;
                    i++;
                }else
                {
                    if (abs(target-sum) < abs(target-ans))
                        ans = sum;
                    j--;
                }
            }
        }
        return ans;
    }
};

```

<br>

----------------------------

##### 18.四数之和

>题目描述：
给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。注意答案中不可以包含重复的四元组。
答案中不可以包含重复的四元组。

示例：

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/4sum

解题思路：先排序，再用四指针法求解，也就是固定第一个和第二个指针，将第三个指针和第四个指针向中间收拢。

时间复杂度：O(NlogN + N^3)

空间复杂度：O(1)


```cpp

class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        if (nums.size() < 4)
            return {};
        vector<vector<int>> ans;
        sort(nums.begin(), nums.end(), std::less<int>());
        for(int m = 0; m < nums.size()-3; m++)
        {
            if (m>0 && nums[m-1]==nums[m])
                continue;
            for(int n = m+1; n < nums.size()-2; n++)
            {
                if (n>m+1 && nums[n-1]==nums[n])
                    continue;

                int i = n + 1, j = nums.size()-1;
                while (i < j)
                {
                    int sum = nums[m] + nums[n] + nums[i] + nums[j];
                    if (sum == target)
                    {
                        ans.push_back(vector<int>{nums[m],nums[n],nums[i++],nums[j--]});
                        while(i<j && nums[i-1]==nums[i]) i++;
                        while(i<j && nums[j+1]==nums[j]) j--;
                    }else if (sum < target)
                    {
                        i++;
                    }else
                    {
                        j--;
                    }
                }
            }
        }
        return ans;
    }
};


```


<br>

-------------------------------

##### 27.移除元素

>题目描述：给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-element

解题思路：此题直接用双指针法解决。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if (nums.empty())
            return 0;
        int i = -1, j = 0;
        for(; j < nums.size(); j++)
        {
            if (nums[j] == val)
                continue;
            else
                nums[++i] = nums[j];
        }
        return i + 1;
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

解题思路：此题实质上属于一次遍历的DFS问题，用DFS递归的方法来进行无回溯的一次遍历，由于中间需要计算阶乘，所以时间复杂度为O(N^2)。

时间复杂度：O(N^2)

空间复杂度：O(N)


```cpp

class Solution {
public:
    int getFactorial(int n)
    {                       
        int res = 1;
        for(int i = n; i >= 1; i--)
            res *= i;
        return res;
    }

    void DFS(vector<int>& nums, int k, vector<int>& visited, string& res, int& level)
    {
        for (int i = 0; i < nums.size(); i++)
        {
            if (visited[i])
                continue;
            int skip = getFactorial(nums.size()-1-level);
            if (k > skip)
            {
                k -= skip;
                continue;
            }
            visited[i] = 1;
            res.push_back(nums[i] + '0');
            level++;
            DFS(nums, k, visited, res, level);
            return;//后面的不用执行
        }
    }

    string getPermutation(int n, int k) {
        vector<int> nums;
        for(int i = 1; i <= n; i++)
            nums.push_back(i);
        int level = 0;
        string res;
        vector<int> visited(n, 0);
        DFS(nums, k, visited, res, level);
        return res;
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
        vector<vector<int>> boxes(9, vector<int>(10, 0));
        for (int i = 0; i < 9; i++)
        {
            for (int j = 0; j < 9; j++)
            {
                if (board[i][j] != '.')
                {
                    int num = board[i][j] - '0';
                    if (rows[i][num] || columns[j][num] || boxes[i/3*3+j/3][num])
                        return false;
                    else
                    {
                        rows[i][num]++;
                        columns[j][num]++;
                        boxes[i/3*3+j/3][num]++;
                    }
                }
            }
        }
        return true;
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

解题思路：双指针法，分别放置左右指针位于数组头尾部，可以让两个指针向中间靠拢，并且让左右两指针的较小值作为蓄水处（另外一个较大值就可以作为蓄水的屏障），设置左最大值和右最大值，左指针只负责左部的蓄水，右指针只负责右部的蓄水。

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

---------------------------------
##### 48.旋转图像

>题目描述：给定一个 n × n 的二维矩阵表示一个图像。
将图像顺时针旋转 90 度。
你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-image


解题思路：暴力法，先对矩阵进行转置，再交换列即可。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for(int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++)
                swap(matrix[i][j], matrix[j][i]);
        for (int j = 0; j < n/2; j++)
            for(int i = 0; i < n; i++)
                swap(matrix[i][j], matrix[i][n-1-j]);
    }
};

```




<br>

-----------------------------------
##### 66.加一
>题目描述：给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。
最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
你可以假设除了整数 0 之外，这个整数不会以零开头。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/plus-one

解题思路：该题从后往前一次遍历即可求得。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n = digits.size();
        int i = n-1;
        for (; i >= 0; i--)
        {
            if (9 == digits[i])
                digits[i] = 0;
            else
            {
                digits[i]++;
                break;
            }
        }
        if (-1 == i && digits[0] == 0)
            digits.insert(digits.begin(), 1);
        return digits;
    }
};

```


<br>

---------------------------------
##### 70.爬楼梯
>题目描述：假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/climbing-stairs


解题思路：经典动态规划题，dp[i] = dp[i-1] + dp[i-2]，可以用滚动数组的方式优化空间复杂度

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (1 == n)
            return 1;
        if (2 == n)
            return 2;
        int fx1 = 1, fx2 = 2;
        int fx;
        for (int i = 3; i <= n; i++)
        {
            fx = fx1 + fx2;
            fx1 = fx2;
            fx2 = fx;
        }
        return fx;
    }
};

```


---------------------------------
##### 89.格雷编码
>题目描述：格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。
给定一个代表编码总位数的非负整数 n，打印其格雷编码序列。即使有多个不同答案，你也只需要返回其中一种。
格雷编码序列必须以 0 开头。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/gray-code

解题思路：设置一个标志位pos（pos初始值为 1），pos在每一次遍历中都需要左移一位，然后再加到返回值的vector数组里，每一次的遍历都通过pos相加创建新的数，然后再将这些数加到返回值数组里面。

时间复杂度：O(2^n)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> ans{0};
        int pos = 1;
        for (int i = 1; i <= n; i++)
        {
            vector<int> temp = ans;
            for (auto it = temp.rbegin(); it != temp.rend(); it++)
            {
                *it += pos;
                ans.push_back(*it);
            }
            pos <<= 1;
        }
        return ans;
    }
};

```

<br>

--------------------
##### 73.矩阵置零
>题目描述：给定一个 m x n 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用原地算法（要求空间复杂度优化为O(1)）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/set-matrix-zeroes

* **解法一**

解题思路：暴力法，将整个矩阵遍历，若某一个元素为0则将该元素所在行与列置零，最坏的情况也就是矩阵中所有的元素均为0。

时间复杂度：O(M*N*(M+N))

空间复杂度：O(1)

* **解法二**

解题思路：在暴力法上进行改进，将第一行与第一列作为置零的标志位，首先判断第一行与第一列是否需要置零，再从第二行第二列开始查找0并设置标志位，然后再从第二行第二列开始遍历，并根据设置的标志位判断是否需要置零，最后再判断第一行与第一列是否需要置零。

时间复杂度：O(M*N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        if (matrix.empty())
            return;
        int m = matrix.size(), n = matrix[0].size();
        bool isRow = false, isColumn = false;
        //判断第一行第一列是否需要置零
        for (int i = 0; i < m; i++)
            if (0 == matrix[i][0])
            {
                isColumn = true;
                break;
            }
        for (int j = 0; j < n; j++)
            if (0 == matrix[0][j])
            {
                isRow = true;
                break;
            }
        //设置标志位
        for (int i = 1; i < m; i++)
            for (int j = 1; j < n; j++)
                if (0 == matrix[i][j])
                {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
        //置零
        for (int i = 1; i < m; i++)
            for (int j = 1; j < n; j++)
                if (0==matrix[i][0] || 0==matrix[0][j])
                    matrix[i][j] = 0;
        //第一行第一列置零
        if (isRow)
            for (int j = 0; j < n; j++)
                matrix[0][j] = 0;
        if (isColumn)
            for (int i = 0; i < m; i++)
                matrix[i][0] = 0;
    }
};

```

<br>

-----------------------
##### 134.加油站
>题目描述：在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。
你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。
如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。
说明: 
如果题目有解，该答案即为唯一答案。
输入数组均为非空数组，且长度相同。
输入数组中的元素均为非负数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/gas-station

解题思路：此题的gas[i]-cost[i]可看成一体，先遍历一次，若sum>0则够环视一周，反之返回-1；设置一个start点，该点处的 gas[i]-cost[i]必须大于0，并且要求能够不断存储sum油量走到最后，那么该start点就可以作为初始点。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        if (gas.empty())
            return -1;

        int sum = 0;
        for (int i = 0; i < gas.size(); i++)
            sum += gas[i] - cost[i];
        if (sum < 0)
            return -1;
        int start = 0;
        sum = 0;
        for (int i = 0; i < gas.size(); i++)
        {
            sum  += gas[i] - cost[i];
            if (sum  < 0)
            {
                start = i+1;
                sum = 0;
            }
        }
        return start;
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

-------------------------------------------
##### 136.只出现一次的数字
>题目描述：给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/single-number

解题思路：对整个数组中的元素进行异或操作，最后剩下的值就是只出现一次的元素。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (auto& i : nums)
            ans ^= i;
        return ans;
    }
};
```

<br>

-------------------------------------------
##### 137.只出现一次的数字2
>题目描述：给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/single-number-ii

解题思路：位运算，每个数都有32位，从最低位0位开始，遍历数组累积该位上的数在%3，也就求得了该位上有用的数，按照此方法遍历32位再累加即可求得只出现了一次的元素。该方法同样可以求解类似问题：有一个元素出现了一次，其余元素出现了k（k为奇数且不为1）次。

时间复杂度：O(32*N) ≈ O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int i = 0; i < 32; i++)
        {
            int cnt = 0;
            for (auto& e: nums)
                if (e & (1<<i))
                    cnt++;
            cnt %= 3;
            if (cnt)
                ans += (1<<i);
        }
        return ans;
    }
};

```

<br>

----------------------------
##### 260.只出现一次的数字3
>题目描述：给定一个整数数组 nums，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。
结果输出的顺序并不重要。
你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/single-number-iii

解题思路：先对整个数组进行异或操作，最后剩下的元素是求解的两个元素异或的结果，找到该结果中为1的某一位，通过该位将整个数组分为两个部分，每个部分各自进行异或操作也就可以得到求解的两个元素了。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int target = 0;
        for (auto& e: nums)
            target ^= e;
        int i = 1;
        while (!(i&target))
            i <<= 1;
        int res1 = 0, res2 = 0;
        for (auto& e: nums)
        {
            if (i & e)
                res1 ^= e;
            else
                res2 ^= e;
        }
        return vector<int>{res1, res2};
    }
};

```

<br>

---------------------------
##### 剑指 Offer 03. 数组中重复的数字
>题目描述:找出数组中重复的数字。
在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof

解题思路：采用原地哈希法，也就是下标和对应的值必须相同，不然就需要不断的交换。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        if (nums.empty())
            return -1;        
        for (int i = 0; i < nums.size(); i++)
        {
            if (i != nums[i])
            {
                if (nums[i] == nums[nums[i]])
                    return nums[i];
                swap(nums[i], nums[nums[i]]);
            }
        }
        return -1;
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

