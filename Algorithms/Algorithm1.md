- [一.数组专题](#一数组专题)
    - [88.合并两个有序数组](#88合并两个有序数组)
    - [27.移除元素](#27移除元素)
    - [26.删除排序数组中的重复项](#26删除排序数组中的重复项)
    - [延伸.删除排序数组中的重复项](#延伸删除排序数组中的重复项)
    - [80.删除排序数组中的重复项2](#80删除排序数组中的重复项2)
    - [剑指 Offer 39. 数组中出现次数超过一半的数字](#剑指-offer-39-数组中出现次数超过一半的数字)
    - [剑指 Offer 57. 和为s的两个数字](#剑指-offer-57-和为s的两个数字)
    - [剑指 Offer 57 - II. 和为s的连续正数序列](#剑指-offer-57---ii-和为s的连续正数序列)
    - [1.两数之和](#1两数之和)
    - [15.三数之和](#15三数之和)
    - [16.最接近的三数之和](#16最接近的三数之和)
    - [18.四数之和](#18四数之和)
    - [494. 目标和](#494-目标和)
    - [11.盛最多水的容器](#11盛最多水的容器)
    - [42.接雨水](#42接雨水)
    - [84.柱状图中最大的矩形](#84柱状图中最大的矩形)
    - [85.最大矩阵](#85最大矩阵)
    - [面试题 17.24. 最大子矩阵](#面试题-1724-最大子矩阵)
    - [53.最大子序和](#53最大子序和)
    - [128.最长连续序列](#128最长连续序列)
    - [300. 最长递增子序列](#300-最长递增子序列)
    - [134.加油站](#134加油站)
    - [135.分发糖果](#135分发糖果)
    - [48.旋转图像](#48旋转图像)
    - [73.矩阵置零](#73矩阵置零)
    - [54.螺旋矩阵](#54螺旋矩阵)
    - [59.螺旋矩阵2](#59螺旋矩阵2)
    - [74.搜索二维矩阵](#74搜索二维矩阵)
    - [剑指 Offer 03. 数组中重复的数字](#剑指-offer-03-数组中重复的数字)
    - [剑指 Offer 53 - II. 0～n-1中缺失的数字](#剑指-offer-53---ii-0n-1中缺失的数字)
    - [41.缺失的第一个正数](#41缺失的第一个正数)
    - [1539. 第 k 个缺失的正整数](#1539-第-k-个缺失的正整数)
    - [剑指 Offer 61. 扑克牌中的顺子](#剑指-offer-61-扑克牌中的顺子)


### 一.数组专题


---------------------------
##### 88.合并两个有序数组
>题目描述:
给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。
说明：
初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-sorted-array

解题思路：双指针，两个指针从后往前移动即可。

时间复杂度：O(M+N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m-1, j = n-1, k = m+n-1;
        while (0<=i && 0<=j)
        {
            if (nums1[i] >= nums2[j])
                nums1[k--] = nums1[i--];
            else
                nums1[k--] = nums2[j--];
        }
        while (0 <= i)
            nums1[k--] = nums1[i--];
        while (0 <= j)
            nums1[k--] = nums2[j--];
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
        int i = 0, j = 0;
        while (j < nums.size())
        {
            if (nums[j] == val)
                ++j;
            else
                nums[i++] = nums[j++];
        }
        return i;
    }
};


```

<br>


---------------------------
##### 26.删除排序数组中的重复项
>题目描述：给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。
延伸：如果是元素只要重复了，那么就将该重复的元素全部删除，这样的话又该怎么做呢？

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
        if (nums.size() < 2)
            return nums.size();
        int i = 0, j = 1;
        while (j < nums.size())
        {
            if (nums[i] == nums[j])
                ++j;
            else
                nums[++i] = nums[j++];
        }
        return i + 1;
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


---------------------------
##### 延伸.删除排序数组中的重复项
>题目描述：如果是元素只要重复了，那么就将该重复的元素全部删除，这样的话又该怎么做呢？

* **解法**

解题思路：双指针法，慢指针用于存储不重复的元素，快指针用于跳过重复的元素（快指针主要是跟指向的下一个元素比较）

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int removeDuplicates(vector<int>& nums) 
{
    if (nums.size() < 2)
        return nums.size();
    int i = 0, j = 0;
    while (j < nums.size())
    {
        if (j == nums.size()-1)
        {
            nums[i++] = nums[j++];
        }
        else if (nums[j] == nums[j+1])
        {
            int val = nums[j];
            while (j < nums.size() && nums[j] == val)
                ++j;
        }else
            nums[i++] = nums[j++];
    }
    return i;
}

int main()
{
    vector<int> nums{0,0,1,1,2,2,5};
    int n = removeDuplicates(nums);
    for (int i = 0; i < n; ++i)
        cout << nums[i] << " ";
    cout << endl;
    return 0;
}
```

<br>


----------------------
##### 80.删除排序数组中的重复项2
>题目描述：给定一个增序排列数组 nums ，你需要在 原地 删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii

解题思路：该题用双指针法解决，需要判断慢指针前面的情况，分为这三种情况：快指针和慢指针值不同，快慢指针值相同并且慢指针前后值相同，快慢指针值不同且慢指针前后值不同。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() <= 2)
            return nums.size();
        int i = 1, j = 2;
        while (j < nums.size())
        {
            if (nums[i] == nums[j])
            {
                if (nums[i-1] == nums[i])
                    ++j;
                else
                    nums[++i] = nums[j++];
            }else
                nums[++i] = nums[j++];  
        }
        return i + 1;
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

解题思路：投票法，只需要进行一次遍历即可。先找出出现频率最高的数字，再判断该数字的出现频率是否超过数组的一半。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) 
    {   
        int num = nums[0], cnt = 1;
        for (int i = 1; i < nums.size(); ++i)
        {
            if (num == nums[i])
            {
                ++cnt;
            }else
            {
                --cnt;
                if (0 == cnt)
                {
                    num = nums[i];
                    cnt = 1;
                }
            }
        }
        cnt = 0;
        for (const auto& e: nums)
            if (num == e)
                cnt ++;
        return cnt >= (double)nums.size()/2 ? num : -1;
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


解题思路：和上一题的方法相同，只是这里可以不用去重，唯一需要注意的是返回这三个数的和！

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

---------------------------
##### 494. 目标和
>题目描述:给你一个整数数组 nums 和一个整数 target 。
向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：
例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。
示例 1：
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
示例 2：
输入：nums = [1], target = 1
输出：1

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/target-sum

解题思路：用DFS遍历所有的情况

时间复杂度：O(2^N)

空间复杂度：O(N)

```cpp
class Solution {
    int ans = 0;
public:
    void DFS(vector<int>& nums, int target, int idx, int sum)
    {
        if (idx == nums.size())
        {
            if (sum == target)
                ans++;
        }else
        {
            DFS(nums, target, idx + 1, sum + nums[idx]);            
            DFS(nums, target, idx + 1, sum - nums[idx]);            
        }
        
    }

    int findTargetSumWays(vector<int>& nums, int target) {
        DFS(nums, target, 0, 0);
        return ans;
    }
};

```

<br>




---------------------------
##### 11.盛最多水的容器
>题目描述:给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
说明：你不能倾斜容器。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/container-with-most-water

解题思路：用双指针向中间靠拢，选两个容器之间较小的向中间靠拢，找到更大的容器就停下。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0, r = height.size()-1;
        int ans = 0;
        while (l < r)
        {
            if (height[l] <= height[r])
            {
                ans = std::max(ans, (r-l) * height[l]);
                ++l;
            }
            else 
            {
                ans = std::max(ans, (r-l) * height[r]);
                --r;
            }
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

解题思路：双指针法，分别放置左右指针位于数组头尾部，可以让两个指针向中间靠拢，并且让左右两指针的较小值作为蓄水处（另外一个较大值就可以作为蓄水的屏障，且保证较大值一定比另一边连续的值都大），设置左最大值和右最大值并不断更新。

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
                ans += lMax - height[l]; 
                l++;
            }
            else
            {
                rMax = std::max(rMax, height[r]);
                ans += rMax - height[r];
                r--;
            }
        }
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

空间复杂度：O(M*N)

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

    int maximalRectangle(vector<vector<char>>& matrix) {
        if (matrix.empty())
            return 0;
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
            {
                if ('0' == matrix[i][j])
                    dp[i][j] = 0;
                else
                {
                    if (0 == i)
                        dp[i][j] = 1;
                    else
                        dp[i][j] = dp[i-1][j] + 1;
                }
            }
        
        int ans = 0;
        for (int i = 0; i < m; ++i)
            ans = std::max(ans, largestRectangleArea(dp[i]));

        return ans;
    }
};

```


<br>

---------------------------
##### 面试题 17.24. 最大子矩阵
>题目描述：给定一个正整数、负整数和 0 组成的 N × M 矩阵，编写代码找出元素总和最大的子矩阵。leetcode要求如下：


解题思路：最大子序和的变形题，dp[i]代表着矩阵第j列从上到下的累加和,sum代表每一次的局部累加最大值。

时间复杂度：O(M^2*N)

空间复杂度：O(N)


来源：牛客,只要求求出最大和
链接：https://www.nowcoder.com/questionTerminal/a5a0b05f0505406ca837a3a76a5419b3?commentTags=C%2FC%2B%2B

```cpp
int getMaxMatrix(vector<vector<int>>& matrix) 
{   
    int m = matrix.size(), n = matrix[0].size();
    vector<int> dp(n, 0);
    int ans = 0, sum = 0;
    for (int up = 0; up < m; ++up)
    {
        std::fill(dp.begin(), dp.end(), 0);
        for (int i = up; i < m; ++i)
        {
            sum = 0;
            for (int j = 0; j < n; ++j)
            {
                dp[j] += matrix[i][j];
                sum = std::max(sum + dp[j], dp[j]);
                ans = std::max(ans, sum);
            }
        }
    }
    return ans;
}


```

来源：力扣（LeetCode）返回一个数组 [r1, c1, r2, c2]，其中 r1, c1 分别代表子矩阵左上角的行号和列号，r2, c2 分别代表右下角的行号和列号。若有多个满足条件的子矩阵，返回任意一个均可。
链接：https://leetcode-cn.com/problems/max-submatrix-lcci/submissions/

```cpp
vector<int> getMaxMatrix(vector<vector<int>>& matrix) 
{   
    vector<int> ans(4, 0);
    int m = matrix.size(), n = matrix[0].size();
    vector<int> dp(n, 0);
    int maxSum = INT32_MIN;
    int r1 = 0, c1 = 0, r2 = 0, c2 = 0;
    for (int up = 0; up < m; ++up)
    {
        std::fill(dp.begin(), dp.end(), 0);
        for (int i = up; i < m; ++i)
        {
            int sum = 0;
            for (int j = 0; j < n; ++j)
            {
                dp[j] += matrix[i][j];

                if (sum + dp[j] <= dp[j])
                {
                    sum = dp[j];
                    r1 = up, c1 = j;
                }else
                    sum += dp[j];
                
                if (maxSum < sum)
                {
                    maxSum = sum;
                    r2 = i, c2 = j;
                    ans[0] = r1, ans[1] = c1, ans[2] = r2, ans[3] = c2;
                }
            }
        }
    }
    return ans;
}


```


<br>



---------------------------
##### 53.最大子序和
>题目描述:给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-subarray

解题思路：动态规划，设置dp数组记录当前较为大的子序和，最大的子序和会在过程中出现，用一个ans做记录即可。
dp[i] = std::max(dp[i-1]+nums[i], nums[i]), 将动态规划数组优化为只有两个即 dp[0] 和 dp[1]

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int ans = nums[0], dp0 = nums[0], dp1 = nums[0];
        for (int i = 1; i < n; ++i)
        {
            dp1 = std::max(dp0 + nums[i], nums[i]);
            ans = std::max(ans, dp1);
            dp0 = dp1;
        }
        return ans;
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

* **解法一**

解题思路：采用中心扩散的思想，先用hashset将所有元素存储，以任意一点为中心不断向两边扩散，同时需要一遍扩散一边删除hashset中的元素，这样可以保证只有一次遍历。

时间复杂度：O(N)

空间复杂度：O(N)


```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> hashset;
        for (const auto& e: nums)
            hashset.insert(e);
        int maxLen = 0;
        while (!hashset.empty())
        {
            int cur = (*hashset.begin()), l = cur - 1, r = cur + 1;
            hashset.erase(cur);
            while (hashset.find(l) != hashset.end())
            {
                hashset.erase(l);
                --l;
            }
            while (hashset.find(r) != hashset.end())
            {
                hashset.erase(r);
                ++r;
            }
            maxLen = std::max(maxLen, r - l - 1);
        }
        return maxLen;
    }
};

```

* **解法二**


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



--------------------------
##### 300. 最长递增子序列
>题目描述：给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。
子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。
进阶：
你可以设计时间复杂度为 O(n^2) 的解决方案吗？
你能将算法的时间复杂度降低到 O(n log(n)) 吗?

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-increasing-subsequence

* **解法一**

解题思路：动态规划，dp[i]代表到位置i为止的最长子序列长度。dp[i] = std::max(dp[i], dp[j] + 1);

时间复杂度：O(N^2)

空间复杂度：O(N)


```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        int maxLen = 0;
        vector<int> dp(n, 0);
        for (int i = 0; i < n; ++i)
        {
            dp[i] = 1;
            for (int j = 0; j < i; ++j)
            {
                if (nums[j] < nums[i])
                    dp[i] = std::max(dp[i], dp[j] + 1);
            }
            maxLen = std::max(maxLen, dp[i]);
        }
        return maxLen;
    }
};

```


* **解法二**

解题思路：贪心+二分法，dp[i]代表长度为i的最长上升子序列末尾元素的最小值。并且用len记录目前最长上升子序列的长度

时间复杂度：O(NlogN)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty())
            return 0;
        int n = nums.size();
        int len = 1;
        vector<int> dp(n + 1, 0);
        dp[len] = nums[0];
        for (int i = 1; i < n; ++i)
        {
            if (nums[i] > dp[len])
                dp[++len] = nums[i];
            else
            {
                int l = 1, r = len;
                int k = 0;
                while (l <= r)
                {
                    int mid = (l + r) >> 1;
                    if (dp[mid] < nums[i])
                    {
                        k  = mid;
                        l = mid + 1;
                    }else
                    {
                        r = mid - 1;
                    }
                }
                dp[k + 1] = nums[i];                
            }
        }
        return len;
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
        vector<int> l(n, 1), r(n, 1);
        for (int i = 1; i < n; ++i)
            if (ratings[i] > ratings[i-1])
                l[i] = l[i-1] + 1;
        for (int i = n - 2; i >= 0; --i)
            if (ratings[i] > ratings[i+1])
                r[i] = r[i+1] + 1;
        int sum = 0;
        for (int i = 0; i < n; ++i)
            sum += std::max(l[i], r[i]);
        return sum;
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


解题思路：暴力法，先对矩阵进行转置，再交换列即可。如果是逆时针90度，那么先转置，再交换行。

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

解题思路：第一次先遍历 M*N 整个数组，过程中将第一行与第一列作为为标志位，并记录下第一行与第一列中是否本身就有0的存在；第二次遍历从下标1开始进行，若行列的某一个标志位为0的话就置零；第三次遍历看是否需要将第一行第一列置零。

时间复杂度：O(M*N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        
        int m = matrix.size(), n = matrix[0].size();
        bool isRow = false, isCol = false; //        
        //第一次遍历，记录第一行第一列的标志位
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (0 == matrix[i][j])
                {
                    if (0 == i)
                        isRow = true;
                    if (0 == j)
                        isCol = true;
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
        //第二次遍历，根据标志位将矩阵置零
        for (int i = 1; i < m; ++i)
            for (int j = 1; j < n; ++j)
                if (0==matrix[0][j] || 0==matrix[i][0])
                    matrix[i][j] = 0;
        //第三次，将第一行第一列置零
        if (isRow)
        {
            for (int j = 0; j < n; ++j)
                matrix[0][j] = 0;
        }
        if (isCol)
        {
            for (int i = 0; i < m; ++i)
                matrix[i][0] = 0;
        }
    }
};
```

<br>



---------------------------
##### 54.螺旋矩阵
>题目描述:给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/spiral-matrix

解题思路：设置上下左右边界即可。

时间复杂度：O(M*N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.empty())
            return {};
        vector<int> ans;
        int M = matrix.size(), N = matrix[0].size();
        int up = 0, down = M-1, l = 0, r = N-1;//设置上下左右边界
        while (1)
        {
            for (int j = l; j <= r; j++)
                ans.push_back(matrix[up][j]);
            if (++up > down)
                break;
            for (int i = up; i <= down; i++)
                ans.push_back(matrix[i][r]);
            if (--r < l)
                break;
            for (int j = r; j >= l; j--)
                ans.push_back(matrix[down][j]);
            if (--down < up)
                break;
            for (int i = down; i >= up; i--)
                ans.push_back(matrix[i][l]);
            if (++l > r)
                break;
        }
        return ans;
    }
};

```

<br>

---------------------------
##### 59.螺旋矩阵2
>题目描述:给定一个正整数 n，生成一个包含 1 到 n^2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/spiral-matrix-ii

解题思路：螺旋遍历的方法，与上一题解法相同。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> matrix(n, vector<int>(n));
        int up = 0, down = n-1, l = 0, r = n-1;//设置上下左右边界
        int cnt = 0;
        while (1)
        {
            for (int j = l; j <= r; j++)
                matrix[up][j] = ++cnt;
            if (++up > down)
                break;
            for (int i = up; i <= down; i++)
                matrix[i][r] = ++cnt;
            if (--r < l)
                break;
            for (int j = r; j >= l; j--)
                matrix[down][j] = ++cnt;
            if (--down < up)
                break;
            for (int i = down; i >= up; i--)
                matrix[i][l] = ++cnt;
            if (++l > r)
                break;
        }
        return matrix;
    }
};

```

<br>




---------------------------
##### 74.搜索二维矩阵
>题目描述:编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：
每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-a-2d-matrix

* **解法一**

解题思路：从右上角开始寻找，这样可以找准单一变量，向左减小，向下增大。

时间复杂度：O(M+N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int i = 0, j = n - 1;
        while (i < m && j >= 0)
        {
            if (matrix[i][j] == target)
                return true;
            else if (matrix[i][j] > target)
                --j;
            else
                ++i;
        }
        return false;
    }
};


```


* **解法二**

解题思路：将二维当做一维，直接进行二分法。

时间复杂度：O(log(M*N))

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty())
            return false;
        int m = matrix.size(), n = matrix[0].size();
        int l = 0, r = m * n - 1;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (matrix[mid/n][mid%n] == target)
                return true;
            else if (matrix[mid/n][mid%n] > target)
                r = mid - 1;
            else 
                l = mid + 1;
        }
        return false;
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
##### 剑指 Offer 53 - II. 0～n-1中缺失的数字
>题目描述：在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。该数组从0开始递增。
示例 1:
输入: [0,1,3]
输出: 2
示例 2:
输入: [0,1,2,3,4,5,6,7,9]
输出: 8

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof

解题思路：递增数组，二分查找，充分利用数组有序，下标与元素对应这两个条件。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        if (nums.empty())
            return 0;
        int l = 0, r = nums.size()-1;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (mid == nums[mid])
                l = mid + 1;
            else if (mid != nums[mid])
                r = mid - 1;
        }
        return l;
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

----------------------------------
##### 1539. 第 k 个缺失的正整数
>题目描述：给你一个 严格升序排列 的正整数数组 arr 和一个整数 k 。
请你找到这个数组里第 k 个缺失的正整数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/kth-missing-positive-number


解题思路：进行一次遍历计数即可

时间复杂度：O(N) N代表的是arr.back()

空间复杂度：O(1)


```cpp
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {
        if (arr.empty())
            return k;
        int cnt = 0;
        int i = 0;
        for (int num = 1; num < arr.back(); ++num)
        {
            if (num == arr[i])
            {
                ++i;
            }else
            {
                cnt++;
                if (cnt == k)
                    return num;
            }
        }
        return arr.back() + (k - cnt);
    }
};


* **解法二**

解题思路：可以以占位的思想来解决该题，如果nums[i] <= k , 则说明 Nums[i]占用了 k的一个位子，所以k+1

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {
        for (const auto& e: arr)
            if (e <= k)
                ++k;
        return k;        
    }
};

```


---------------------------
##### 剑指 Offer 61. 扑克牌中的顺子
>题目描述:从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
数组长度为 5 
数组的数取值为 [0, 13] .

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof

解题思路：先排序，再判断数组中有无重复元素，再判断前后的元素值的差值即可。

时间复杂度：O(1)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        sort(nums.begin(), nums.end(), std::less<int>());
        for (int i = 0; i < nums.size(); ++i)
        {
            if (0 == nums[i])
                continue;
            if (0<i && nums[i-1]==nums[i])
                return false;
        }
        int i = 0;
        while (0 == nums[i]) ++i;
        return nums.back()-nums[i] <= 4;
    }
};

```

<br>

