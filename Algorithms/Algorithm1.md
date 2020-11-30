


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

-------------------------------
##### 33.搜索旋转排序数组
>题目描述：给你一个整数数组 nums ，和一个整数 target 。
该整数数组原本是按升序排列，但输入时在预先未知的某个点上进行了旋转。（例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] ）。
请你在数组中搜索 target ，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。注意其中每一个值都是独一无二的

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array

解题思路：该题采用二分法求解，每一次二分都需要先判断是否与target相等，再利用两个区间：一个左升序数组的左区间，一个是右升序数组的右区间进行判断即可。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty())
            return -1;
        int l = 0, r = nums.size()-1;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] == target)
                return mid;
            if (nums[l] == nums[mid]) //无法判断mid落在了左升序还是右升序
                l++;
            else if (nums[mid] > nums[l]) //mid位于左升序数组
            {
                if (nums[l]<=target && target<=nums[mid])
                    r = mid - 1;
                else 
                    l = mid + 1;
            }
            else //mid 位于右升序数组
            {
                if (nums[mid]<=target && target<=nums[r])
                    l = mid + 1;
                else 
                    r = mid - 1;
            }
        }
        return -1;
    }
};

```


<br>

-----------------------
##### 81.搜索旋转排序数组2
>题目描述：假设按照升序排序的数组在预先未知的某个点上进行了旋转。
( 例如，数组 [0,0,1,2,2,5,6] 可能变为 [2,5,6,0,0,1,2] )。
编写一个函数来判断给定的目标值是否存在于数组中。若存在返回 true，否则返回 false。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii

解题思路：该题的解决方法与上一题一样，为了方便理解就统一写法，唯一的不同之处在于该题中允许数字重复，那么就在一开始无法判断mid落在左升序还是右升序数组中了。这样可能会造成最坏的O(N)时间复杂度。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
          if (nums.empty())
            return false;
        int l = 0, r = nums.size()-1;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] == target)
                return true;
            if (nums[l] == nums[mid]) //无法判断mid落在了左升序还是右升序
                l++;
            else if (nums[mid] > nums[l]) //mid位于左升序数组
            {
                if (nums[l]<=target && target<=nums[mid])
                    r = mid - 1;
                else 
                    l = mid + 1;
            }
            else //mid 位于右升序数组
            {
                if (nums[mid]<=target && target<=nums[r])
                    l = mid + 1;
                else 
                    r = mid - 1;
            }
        }
        return false;
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

----
##### 128.最长连续序列
>题目描述：给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
要求：你可以设计并实现时间复杂度为 O(n) 的解决方案吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-consecutive-sequence

* **解法一**

解题思路：并查集，下标代表该数字，元素代表从该数字出发能够到达的位置；

时间复杂度：O(N)

空间复杂度：O(N)


```cpp
class Solution {
    unordered_map<int,int> union_find; 
public:
    int find(int x)
    {
        return union_find.count(x) ? union_find[x]=find(union_find[x]) :  x;
    }

    int longestConsecutive(vector<int>& nums) {
        if (nums.empty())
            return 0;
        for(auto& e: nums)
            union_find[e] = e + 1;
        
        int Max = INT32_MIN;
        for(auto& beg: nums)
        {
            int end = find(beg+1);
            Max = std::max(end-beg, Max);
        }
        return Max;
    }
};

```

* **解法二**

解题思路：采用常规遍历+哈希表方法，相比于第一种的并查集方法，该方法会超出时间限制，原因是有了大量的重复计算操作

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty())
            return 0;
        unordered_map<int,int> hashmap;
        for(auto& i : nums)
            hashmap[i] = i;
        int Max = INT32_MIN;
        for(auto i: nums)
        {
            int cnt = 1;
            while (hashmap.count(++i))
                cnt++;
            Max = std::max(Max, cnt);
        }
        return Max;
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

解题思路：

时间复杂度：

空间复杂度：

```cpp
17:55

```

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------------

48.旋转图像
66.加一
70.爬楼梯
89.格雷编码
73.矩阵置零
134.加油站
135.分发糖果
136.只出现一次的数字
137.只出现一次的数字2

### 链表

2.两数相加
92.反转链表2
86.分隔链表
83.删除排序链表中的重复元素
82.删除排序链表中的重复元素2
61.旋转链表
19.删除链表的倒数第N个节点
24.两两交换链表中的节点
25.K个一组翻转链表
138.复制带随机指针的链表
141.环形链表
142.环形链表2
143.重排链表
146.LRU缓存机制



