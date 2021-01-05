- [六.查找专题](#六查找专题)
        - [33. 搜索旋转排序数组](#33-搜索旋转排序数组)
        - [81. 搜索旋转排序数组 II](#81-搜索旋转排序数组-ii)
        - [153. 寻找旋转排序数组中的最小值](#153-寻找旋转排序数组中的最小值)
        - [154. 寻找旋转排序数组中的最小值 II](#154-寻找旋转排序数组中的最小值-ii)
        - [剑指 Offer 53 - II. 0～n-1中缺失的数字](#剑指-offer-53---ii-0n-1中缺失的数字)
        - [34. 在排序数组中查找元素的第一个和最后一个位置](#34-在排序数组中查找元素的第一个和最后一个位置)
        - [35.搜索插入位置](#35搜索插入位置)
        - [74.搜索二维矩阵](#74搜索二维矩阵)



# 七.查找专题

---------------------------
##### 33. 搜索旋转排序数组
>题目描述:升序排列的整数数组 nums 在预先未知的某个点上进行了旋转（例如， [0,1,2,4,5,6,7] 经旋转后可能变为 [4,5,6,7,0,1,2] ）。
请你在数组中搜索 target ，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。
每一个数都是独一无二的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array

解题思路：二分法，先判断nums[mid]是否等待target，再根据最左边和最右边的两个递增区间判断二分。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size()-1;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] == target)
                return mid;
            if (nums[mid] == nums[l]) //无法判断落在左递增区间还是右递增区间
                l++;
            else if (nums[mid] > nums[l])//落在左递增区间
            {
                if (nums[l]<=target && target<=nums[mid])
                    r = mid -1;
                else
                    l = mid + 1;
            }else//落在右递增区间
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

---------------------------
##### 81. 搜索旋转排序数组 II
>题目描述:假设按照升序排序的数组在预先未知的某个点上进行了旋转。
( 例如，数组 [0,0,1,2,2,5,6] 可能变为 [2,5,6,0,0,1,2] )。
编写一个函数来判断给定的目标值是否存在于数组中。若存在返回 true，否则返回 false。
可能会有相同的数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii

解题思路：与上一题的解法相同

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int l = 0, r = nums.size()-1;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] == target)
                return true;
            if (nums[mid] == nums[l]) //无法判断落在左递增区间还是右递增区间
                l++;
            else if (nums[mid] > nums[l])//落在左递增区间
            {
                if (nums[l]<=target && target<=nums[mid])
                    r = mid -1;
                else
                    l = mid + 1;
            }else//落在右递增区间
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

---------------------------
##### 153. 寻找旋转排序数组中的最小值
>题目描述:假设按照升序排序的数组在预先未知的某个点上进行了旋转。例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] 。
请找出其中最小的元素。
nums 中的所有整数都是 唯一 的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array

解题思路：二分法，每一次先判断nums[mid]处于左边的递增区间还是右边的递增区间。若处在左边的递增区间，取最小值后右移；若处在右边的递增区间，取最小值后左移。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size()-1;
        int ans = INT32_MAX;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] >= nums[l]) //mid在左边递增区间
            {
                ans = std::min(ans, nums[l]);
                l = mid + 1;
            }
            else //mid在右边递增区间
            {
                ans = std::min(ans, nums[mid]);
                r = mid - 1;
            }
        }
        return ans;
    }
};

```

<br>


---------------------------
##### 154. 寻找旋转排序数组中的最小值 II
>题目描述:假设按照升序排序的数组在预先未知的某个点上进行了旋转。
( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。
请找出其中最小的元素。
注意数组中可能存在重复的元素。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii

解题思路：该题解法与上一题相同，只是多了一个条件处理的情况。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size()-1;
        int ans = INT32_MAX;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] == nums[l]) //无法判断mid在哪一个递增区间
            {
                ans = std::min(ans, nums[l]);
                l++;
            }
            else if (nums[mid] > nums[l]) //mid在左边递增区间
            {
                ans = std::min(ans, nums[l]);
                l = mid + 1;
            }
            else //mid在右边递增区间
            {
                ans = std::min(ans, nums[mid]);
                r = mid - 1;
            }
        }
        return ans;
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
##### 34. 在排序数组中查找元素的第一个和最后一个位置
>题目描述:
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
如果数组中不存在目标值 target，返回 [-1, -1]。
进阶：
你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？

解题思路：本题可以通过实现 lower_bound 和 upper_bound函数来实现，也就是用二分法求开始位置和结束位置。

时间复杂度：O(logn)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int lower_bound(vector<int>& nums, int target)
    {
        int l = 0, r = nums.size();
        while (l < r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] < target)
                l = mid + 1;
            else if (nums[mid] > target)
                r = mid;
            else
                r = mid;
        }
        return l;
    }

    int upper_bound(vector<int>& nums, int target)
    {
        int l = 0, r = nums.size();
        while (l < r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] < target)
                l = mid + 1;
            else if (nums[mid] > target)
                r = mid;
            else
                l = mid+1;
        }
        return r-1;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        if (nums.empty() || target<nums.front() || target>nums.back())
            return vector<int>{-1, -1};
        int l = lower_bound(nums, target);
        int r = upper_bound(nums, target);
        if (l <= r)
            return vector<int>{l, r};
        else
            return vector<int>{-1, -1};
    }
};

```

<br>


---------------------------
##### 35.搜索插入位置
>题目描述:给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
你可以假设数组中无重复元素。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-insert-position

解题思路：用二分法可以找到该目标值下标，或者说是插入的顺序。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l = 0, r = nums.size()-1;
        while (l <= r)
        {
            int mid = (l + r) >> 1;
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                l = mid + 1;
            else 
                r = mid - 1;
        }
        return l;
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
