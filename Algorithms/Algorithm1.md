- [一.线性表专题](#一线性表专题)
    - [线性表专题：数组系列](#线性表专题数组系列)
        - [26.删除排序数组中的重复项](#26删除排序数组中的重复项)
        - [80.删除排序数组中的重复项2](#80删除排序数组中的重复项2)
        - [剑指 Offer 39. 数组中出现次数超过一半的数字](#剑指-offer-39-数组中出现次数超过一半的数字)
        - [4.寻找两个正序数组的中位数](#4寻找两个正序数组的中位数)
        - [1.两数之和](#1两数之和)
        - [15.三数之和](#15三数之和)
        - [16.最接近的三数之和](#16最接近的三数之和)
        - [18.四数之和](#18四数之和)
        - [27.移除元素](#27移除元素)
        - [31.下一个排列](#31下一个排列)
        - [46.全排列](#46全排列)
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
    - [线性表专题：链表系列](#线性表专题链表系列)
        - [2.两数相加](#2两数相加)
        - [445.两数相加2](#445两数相加2)
        - [206.反转链表](#206反转链表)
        - [92.反转链表2](#92反转链表2)
        - [86.分隔链表](#86分隔链表)
        - [83.删除排序链表中的重复元素](#83删除排序链表中的重复元素)
        - [82.删除排序链表中的重复元素2](#82删除排序链表中的重复元素2)
        - [61.旋转链表](#61旋转链表)
        - [19.删除链表的倒数第N个节点](#19删除链表的倒数第n个节点)
        - [24.两两交换链表中的节点](#24两两交换链表中的节点)
        - [25.K个一组翻转链表](#25k个一组翻转链表)
        - [138.复制带随机指针的链表](#138复制带随机指针的链表)
        - [141.环形链表](#141环形链表)
        - [142.环形链表2](#142环形链表2)
        - [876.链表的中间节点](#876链表的中间节点)
        - [143.重排链表](#143重排链表)
        - [146.LRU缓存机制](#146lru缓存机制)


# 一.线性表专题

### 线性表专题：数组系列


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
        unordered_map<int,int> hashmap;
        for(int i = 0; i < nums.size(); i++)
        {
            int another = target - nums[i];
            if (hashmap.count(another))
                return vector<int>{i, hashmap[another]};
            else
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
        if (nums.size() < 3)
            return {};
        sort(nums.begin(), nums.end(), std::less<int>());
        vector<vector<int>> ans;
        for(int k = 0; k < nums.size()-2; k++)
        {
            if (nums[k]>0 || (k>0&&nums[k]==nums[k-1]))
                continue;
            int i = k + 1, j = nums.size()-1;
            while (i < j)
            {
                int sum = nums[k] + nums[i] + nums[j];
                if (sum == 0)
                {
                    ans.push_back(vector<int>{nums[k], nums[i++], nums[j--]});
                    while(i<j && nums[i-1]==nums[i])
                        i++;;
                    while(i<j && nums[j+1]==nums[j])
                        j--;
                }
                else if (sum < 0)
                    i++;
                else 
                    j--;
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

14:41

解题思路：和上一题的方法相同，唯一需要注意的是返回这三个数的和！

时间复杂度：O(NlogN+N^2) ≈ O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if (nums.size() < 3)
            return -1;
        sort(nums.begin(), nums.end(), std::less<int>());
        int ans = nums[0] + nums[1] + nums[2];
        for(int k = 0; k < nums.size()-2; k++)
        {
            if (k>0 && nums[k]==nums[k-1])
                continue;
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
                    while(i<j && nums[i-1]==nums[i])
                        i++;
                }    
                else 
                {
                    if (abs(target-sum) < abs(target-ans))
                        ans = sum;
                    j--;
                    while(i<j && nums[j+1] == nums[j])
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

----------------------------
##### 31.下一个排列

>题目描述：实现获取 下一个排列 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。
如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。
必须 原地 修改，只允许使用额外常数空间。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/next-permutation

解题思路：两次遍历再加上局部反转即可；第一次遍历从rbegin出发找到第一个降序的数nums[i]，第二次遍历从rbegin出发找到第一个大于nums[i]的数，交换后从i的位置往后进行反转。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        if (nums.size() <= 1)
            return;
        int i = nums.size()-2;
        while(i>=0 && nums[i]>=nums[i+1])
            i--;
        if (i < 0)
        {
            reverse(nums.begin(), nums.end());
            return;
        }
        int j = nums.size()-1;
        while(nums[j] <= nums[i])
            j--;
        swap(nums[i], nums[j]);
        reverse(nums.begin() + i + 1, nums.end());        
    }
};

```

<br>

-----------------------------
##### 46.全排列
>题目描述：给定一个 没有重复 数字的序列，返回其所有可能的全排列。
示例:
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/permutations

解题思路：直接适用DFS遍历法对所有数进行遍历

时间复杂度：O(N!)

空间复杂度：O(N)


```cpp
class Solution {
    vector<vector<int>> ans;
public:
    void DFS(vector<int>& nums, vector<int>& visited, vector<int>& res)
    {
        if (res.size() == nums.size())
        {
            ans.push_back(res);
            return;
        }

        for(int i = 0; i < nums.size(); i++)
        {
            if (visited[i])
                continue;
            visited[i] = 1;
            res.push_back(nums[i]);
            DFS(nums, visited, res);
            visited[i] = 0;
            res.pop_back();
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        int len = nums.size();
        vector<int> visited(len, 0);
        vector<int> res;
        DFS(nums, visited, res);
        return ans;
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
int singleNumber(vector<int> &nums)
{
    int ans = 0;
    for (int i = 0; i < 32; i++)
    {
        int sum = 0;
        for (auto& e: nums)
            if (e & (1<<i))
                sum++;
        sum %= 3;
        ans ^= (sum<<i);
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


### 线性表专题：链表系列

----------------------------
##### 2.两数相加
>题目描述：给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers

解题思路：设置双指针同时遍历，并设置carry进位，返回一个新的链表。

时间复杂度：O(max(M+N))

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* tail = dummy;
        int carry = 0;
        while (l1 || l2 || carry)
        {
            int val1 = (l1 ? l1->val : 0);
            int val2 = (l2 ? l2->val : 0);
            int sum = val1 + val2 + carry;
            carry = (sum >= 10 ? 1 : 0); 
            ListNode* node = new ListNode(sum%10);
            tail->next = node;
            tail = node;
            l1 = (l1 ? l1->next : nullptr);
            l2 = (l2 ? l2->next : nullptr);
        }
        return dummy->next;
    }
};

```

<br>

--------------------------
##### 445.两数相加2
>题目描述：给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
你可以假设除了数字 0 之外，这两个数字都不会以零开头。
如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers-ii

* **解法一**

解题思路：先分别遍历两个链表，用两个数分别存放两个链表的值，将两个数相加，再创建新的链表存放结果即可。此方法运行无法通过，因为链表过长的情况下是无法用某一个数字进行存储的 ！

时间复杂度：O(M+N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int sum1 = 0, sum2 = 0;
        for (auto p = l1; p; p = p->next)
            sum1 = sum1 * 10 + p->val;
        for (auto p = l2; p; p = p->next)
            sum2 = sum2 * 10 + p->val;
        string s(to_string(sum1 + sum2));
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        for (auto& i: s)
        {
            ListNode* node = new ListNode(i-'0');
            tail->next = node;
            tail = node;
        }
        return dummy.next;
    }
};
```

* **解法二**

解题思路：逆序想到双栈法，分别用两个栈存放两个链表的值，再通过弹栈创建新的链表，注意由于是逆序创建，需要使用头插法创建链表。

时间复杂度：O(M+N)

空间复杂度：O(M+N)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> stk1, stk2;
        for (auto p = l1; p; p=p->next)
            stk1.push(p->val);
        for (auto p = l2; p; p=p->next)
            stk2.push(p->val);
        ListNode dummy(-1);
        ListNode *tail = &dummy;
        int carry = 0;
        while (!stk1.empty() || !stk2.empty() || carry)
        {
            int val1 = (stk1.empty() ? 0 : stk1.top());
            int val2 = (stk2.empty() ? 0 : stk2.top());
            if (!stk1.empty()) stk1.pop();
            if (!stk2.empty()) stk2.pop();
            int sum = carry + val1 + val2;
            carry = (sum>=10 ? 1 : 0);
            //头插法
            ListNode *node = new ListNode(sum%10);
            node->next = dummy.next;
            dummy.next = node;
        }
        return dummy.next;
    }
};
```


<br>

-----------------------------------
##### 206.反转链表
>题目描述：反转一个单链表。
示例:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list

* **解法一**

解题思路：迭代法，一边遍历链表一边使用头插法插入。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode dummy(-1);
        ListNode *cur = head;
        while (cur)
        {
            ListNode *temp = cur->next;
            cur->next = dummy.next;
            dummy.next = cur;
            cur = temp;
        }   
        return dummy.next;
    }
};

```

* **解法二**

解题思路：递归法

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode* ret = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return ret;
    }
};
```


<br>

--------------------------
##### 92.反转链表2
>题目描述：反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。
说明:
1 ≤ m ≤ n ≤ 链表长度。
示例:
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list-ii

解题思路：先遍历到位置m的前一个节点，让链表从此处断开称为两个链表，然后不断将第二个链表中的节点通过头插法加入到第一个链表中。

时间复杂度：O(链表长度)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (!head || !head->next || m==n)
            return head;
        ListNode dummy(-1);
        dummy.next = head;
        ListNode *pre = &dummy;//pre指向第m节点的前一个
        for (int i = 0; i < m-1; i++)
            pre = pre->next;
        ListNode *cur = pre->next, *rHead = pre->next;
        pre->next = nullptr;//将两个链表断开，更方便理解
        for (int i = m; i <= n; i++)
        {
            ListNode* temp = cur->next;
            cur->next = pre->next;
            pre->next = cur;
            cur = temp;
        }
        rHead->next = cur;
        return dummy.next;
    }
};

```

<br>

-----------------------------------
##### 86.分隔链表
>题目描述：给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。
你应当保留两个分区中每个节点的初始相对位置。
示例:
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/partition-list

解题思路：设置左右两个头结点，遍历链表，若元素值小于x则添加到左边链表，大于x则添加到右边链表；最后将两个链表连接起来。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode leftDummy(-1), rightDummy(-1);
        ListNode* leftTail = &leftDummy, *rightTail = &rightDummy;
        for (auto p = head; p; p=p->next)
        {
            if (p->val < x)
            {
                leftTail->next = p;
                leftTail = p;
            }
            else
            {
                rightTail->next = p;
                rightTail = p;
            }
        }
        leftTail->next = rightDummy.next;
        rightTail->next = nullptr;
        return leftDummy.next;
    }
};

```

<br>

--------------------------------------
##### 83.删除排序链表中的重复元素
>题目描述：给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

解题思路：设置头结点，一次遍历链表即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode dummy(-1);
        ListNode *tail = &dummy;
        for (auto p = head; p; p=p->next)
        {
            if (tail==&dummy || tail->val!=p->val)
            {
                tail->next = p;
                tail = p;
            }
            else
                continue;
        }
        tail->next = nullptr;
        return dummy.next;
    }
};

```

<br>

-----------------------------------
##### 82.删除排序链表中的重复元素2
>题目描述：给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii

解题思路：使用一次遍历即可求解，先需要设置一个tail指针通过尾插法用来连接后续没有重复的节点，再在tail指针后面使用 快慢双指针 判断节点是否重复。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        ListNode *s = head;
        while (s)
        {
            ListNode* f = s->next;
            if (f && f->val==s->val)
            {
                while(f && f->val==s->val)
                    f = f->next;
                s = f;
            }   
            else
            {
                tail->next = s;
                tail = s;
                s = s->next;
            } 
        }
        tail->next = nullptr;
        return dummy.next;
    }
};

```

<br>

-----------------------------------
##### 61.旋转链表
>题目描述：给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。
示例 1:
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-list

解题思路：先遍历整个链表将链表的长度len测量出来，并将链表的首尾相连；len-k%len 处断开，即可求得新的链表

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next)
            return head;
        int len = 1;
        auto cur = head;
        for (; cur->next; cur=cur->next)
            len++;
        cur->next = head;//成环
        
        int pos = len - k % len;
        cur = head;
        for (int i = 0; i < pos-1; i++)
            cur = cur->next;
        ListNode* start = cur->next;
        cur->next = nullptr;
        return start;
    }
};

```

<br>

-----------------------------------
##### 19.删除链表的倒数第N个节点
>题目描述：给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
示例：
给定一个链表: 1->2->3->4->5, 和 n = 2.
当删除了倒数第二个节点后，链表变为 1->2->3->5.
给定的 n 保证是有效的。
你能尝试使用一趟扫描实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list

解题思路：快慢指针法，快指针先走n步，接着快慢指针同时走，快指针走到末尾时慢指针即可删除倒数第n个节点。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if (!head  || n<=0)
            return head;
        ListNode dummy(-1);
        dummy.next = head;
        ListNode *s = &dummy, *f = &dummy;
        for (int i = 0; i < n; i++)
            f = f->next;
        for (; f->next ; f=f->next)
            s = s->next;
        s->next = s->next->next;
        return dummy.next;
    }
};
```

<br>

-----------------------------------
##### 24.两两交换链表中的节点
>题目描述：
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
示例 1：
输入：head = [1,2,3,4]
输出：[2,1,4,3]
示例 2：
输入：head = []
输出：[]
示例 3：
输入：head = [1]
输出：[1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/swap-nodes-in-pairs


解题思路：只进行一次遍历即可，设置pre指针，l指针，r指针；pre指针在前，l，r指针指向需要交换的两个节点，不断往后迭代。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode dummy(-1);
        dummy.next = head;
        ListNode* pre = &dummy, *l = pre->next, *r = l->next;
        while  (1)
        {
            l->next = r->next;
            pre->next = r;
            r->next = l;
            
            pre = l;
            l = pre->next;
            if (!l) break;
            r = l->next;
            if (!r) break;
        }
        return dummy.next;
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

-----------------------------------
##### 138.复制带随机指针的链表
>题目描述：给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。
要求返回这个链表的 深拷贝。 
我们用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：
val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/copy-list-with-random-pointer

* **解法一**

解题思路：哈希表作为辅助，key为旧节点的指针，value为新创建的节点的指针，一次遍历先创建新节点并记录哈希表，再一次遍历将随机指针填充上即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        unordered_map<Node*, Node*> hashmap;
        Node dummy(-1);
        Node* tail = &dummy;
        for (auto cur = head; cur ; cur=cur->next)
        {
            Node* node = new Node(cur->val);
            tail->next = node;
            tail = node;
            hashmap[cur] = node;
        }
        for (auto p = head, q=dummy.next; p&&q; p=p->next, q=q->next)
            q->random = hashmap[p->random];
        return dummy.next;
    }
};

```

* **解法二**

解题思路：将深拷贝的节点存放在旧节点的next后，新节点的next又连接原来旧节点的下一个节点，这样可以起到模拟哈希表的作用。第一次遍历先创建新节点，第二次遍历填充新节点的 random指针，第三次遍历将新旧链表分开。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head)
            return nullptr;
        //创建新链表
        auto cur = head;
        while(cur)
        {
            Node* node = new Node(cur->val);
            node->next = cur->next;
            cur->next = node;
            cur = cur->next->next;
        }
        //填充新链表的random指针
        auto p = head, q = head->next;
        while (p && q)
        {
            q->random = (p->random? p->random->next : nullptr);
            p = p->next->next;
            if (q->next)
                q = q->next->next;
        }
        //将 新旧链表拆开
        Node* head2 = head->next;
        p = head, q =head2;
        while (p && q)
        {
            p->next = q->next;
            p = p->next;
            q->next = (p ? p->next : nullptr);
            q = q->next;
        }
        return head2;
    }
};

```

<br>

-----------------------------------
##### 141.环形链表
>题目描述：给定一个链表，判断链表中是否有环。
如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。
如果链表中存在环，则返回 true 。 否则，返回 false 。
你能用 O(1)（即，常量）内存解决此问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle

解题思路：快慢指针法，快指针走的速度是慢指针的两倍，如果两个指针能够重新相遇，那么说明有环；若快指针走到末尾，则说明无环。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (!head)
            return false;
        ListNode* s = head, *f = head;
        do 
        {
            if (!f || !f->next)
                return false;
            s = s->next;
            f = f->next->next;
        }while (s != f);
        return true;
    }
};

```

<br>

-----------------------------------
##### 142.环形链表2
>题目描述：给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。
说明：不允许修改给定的链表。
你是否可以使用 O(1) 空间解决此题？
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle-ii

解题思路：使用快慢指针法，快指针的速度是慢指针的两倍；让两指针相遇时，再将快指针放到初始位置处再让两个指针以相同的速度走，再次相遇的地方即为环的入口。
假设a为环之前的那段路，b为环的长度
快指针走的路程：a+xb，慢指针走的路程:a+yb，相减可知相遇时慢指针相当于走了n圈b的长度，也就是n*b，再+a 也就是等于 a + n*b = a。即可求解

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (!head)
            return head;
        ListNode* s = head, *f = head;
        do 
        {
            if (!f || !f->next)
                return nullptr;
            s = s->next;
            f = f->next->next;
        }while (s != f);
        f = head;
        while (s != f)
        {
            s = s->next;
            f = f->next;
        }
        return s;
    }
};

```

<br>

-----------------------------------
##### 876.链表的中间节点
>题目描述：给定一个头结点为 head 的非空单链表，返回链表的中间结点。
如果有两个中间结点，则返回第二个中间结点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/middle-of-the-linked-list

解题思路：快慢指针法

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode* s = head, *f = head;
        while (1)
        {
            if (!f || !f->next)
                return s;
            s = s->next;
            f = f->next->next;
        }
        return s;
    }
};

```

<br>

-----------------------------------
##### 143.重排链表
>题目描述：给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
示例 1:
给定链表 1->2->3->4, 重新排列为 1->4->2->3.
示例 2:
给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reorder-list

解题思路：先通过找到链表的中点将其分成两个链表，再将右边的链表进行反转，再将翻转之后的右边链表依次加入到左边链表元素的右边一个。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    //寻找中间节点，若有两个中间节点则返回第一个
    ListNode* findMid(ListNode*  head)
    {
        auto s = head, f = head;
        while (f)
        {
            if (!f || !f->next || !f->next->next)
                return s;
            s = s->next;
            f = f->next->next;
        }
        return s;
    }

    //翻转链表，并返回新的首节点
    ListNode* reverseList(ListNode* head)
    {
        ListNode dummy(-1);
        auto cur = head;
        while (cur)
        {
            auto temp = cur->next;
            cur->next = dummy.next;
            dummy.next = cur;
            cur = temp;
        }
        return dummy.next;
    }

    void reorderList(ListNode* head) {
        if (!head || !head->next)
            return;
        ListNode* mid = findMid(head);
        ListNode* rHead = mid->next;
        mid->next = nullptr;
        rHead = reverseList(rHead);
        auto p = head, q = rHead;
        while (p && q)
        {
            ListNode* temp = q->next;
            q->next = p->next;
            p->next = q;
            p = p->next->next;
            q = temp;
        }
    }
};

```

<br>

-----------------------------------
##### 146.LRU缓存机制
>题目描述：运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制 。
实现 LRUCache 类：
LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
你是否可以在 O(1) 时间复杂度内完成这两种操作？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lru-cache

解题思路：list + 哈希表辅助法，用list存储元素值，越靠前代表越先进来的，越靠后代表越后进来的；再用哈希表存储key值，映射list对应的迭代器，这样可以快速的进行删除和转移。这样能够保证get 和 push 都是 O(1)的时间复杂度。

时间复杂度：O(1)

空间复杂度：O(N)


```cpp
class LRUCache {
    int _capacity;
    std::list<pair<int,int>> list;
    std::unordered_map<int, std::list<pair<int,int>>::iterator> hashmap; 
public:
    LRUCache(int capacity) {
        _capacity = capacity;
    }
    
    int get(int key) {
        if (hashmap.count(key))
        {
            auto it = hashmap[key];
            list.splice(list.end(), list, it);
            hashmap[key] = prev(list.end());
            return list.rbegin()->second; 
        }
        else
            return -1;
    }
    
    void put(int key, int value) {
        if (hashmap.count(key))
        {
            auto it = hashmap[key];
            list.splice(list.end(), list, it);
            list.rbegin()->second = value;
            hashmap[key] = prev(list.end());
        }
        else
        {
            if (_capacity == list.size())
            {
                hashmap.erase(list.begin()->first);
                list.pop_front();
            }
            list.push_back(make_pair(key, value));
            hashmap[key] = prev(list.end());
        }
    }
};

```


