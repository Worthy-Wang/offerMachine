- [六.查找专题](#六查找专题)
        - [34. 在排序数组中查找元素的第一个和最后一个位置](#34-在排序数组中查找元素的第一个和最后一个位置)
        - [35.搜索插入位置](#35搜索插入位置)
        - [74.搜索二维矩阵](#74搜索二维矩阵)



# 六.查找专题

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
