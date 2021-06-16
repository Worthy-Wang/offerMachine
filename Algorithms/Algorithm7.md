- [七.查找专题](#七查找专题)
    - [33. 搜索旋转排序数组](#33-搜索旋转排序数组)
    - [81. 搜索旋转排序数组 II](#81-搜索旋转排序数组-ii)
    - [153. 寻找旋转排序数组中的最小值](#153-寻找旋转排序数组中的最小值)
    - [154. 寻找旋转排序数组中的最小值 II](#154-寻找旋转排序数组中的最小值-ii)
    - [34. 在排序数组中查找元素的第一个和最后一个位置](#34-在排序数组中查找元素的第一个和最后一个位置)
    - [剑指 Offer 53 - II. 0～n-1中缺失的数字](#剑指-offer-53---ii-0n-1中缺失的数字)
    - [35.搜索插入位置](#35搜索插入位置)
    - [4.寻找两个正序数组的中位数](#4寻找两个正序数组的中位数)
    - [剑指 Offer 41. 数据流中的中位数](#剑指-offer-41-数据流中的中位数)



### 七.查找专题

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
##### 34. 在排序数组中查找元素的第一个和最后一个位置
>题目描述:
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
如果数组中不存在目标值 target，返回 [-1, -1]。
进阶：
你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/

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
            if (nums[mid] > target)
                r = mid;
            else if (nums[mid] < target)
                l = mid + 1;
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
            if (nums[mid] > target)
                r = mid;
            else if (nums[mid] < target)
                l = mid + 1;
            else
                l = mid + 1;
        }
        return l - 1;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        int low = lower_bound(nums, target);
        int up = upper_bound(nums, target);
        if (low > up)
            return vector<int>{-1, -1};
        
        return vector<int>{low, up};
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



****

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
    double findK(vector<int>& nums1, vector<int>& nums2, int k)
    {
        int i = 0, j = 0, n1 = nums1.size(), n2 = nums2.size();
        while (1)
        {
            if (i == n1)
                return nums2[j + k - 1];
            if (j == n2)
                return nums1[i + k - 1];
            if (1 == k)
                return std::min(nums1[i], nums2[j]);

            int newI = std::min(n1-1, i + k/2 - 1), newJ = std::min(n2-1, j + k/2 - 1);
            if (nums1[newI] < nums2[newJ])
            {
                k -= newI + 1 - i ;
                i = newI + 1;
            }else
            {
                k -= newJ + 1 - j;
                j = newJ + 1;
            }
        }
        return 0;
    }

    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n1 = nums1.size(), n2 = nums2.size(), n = n1 + n2;
        if (n & 0x1)
            return findK(nums1, nums2, n/2 + 1);
        else
            return (findK(nums1, nums2, n/2+1) + findK(nums1, nums2, n/2)) / 2;
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

* **方法一**

解题思路：用vector数组存储添加进行来的元素，保证数组是有序存储的，在插入元素的时候就会进行插入排序。

时间复杂度：addNum:O(N), findMedian : O(1)

空间复杂度： O(N)


```cpp
class MedianFinder {
    vector<int> nums;
public:
    /** initialize your data structure here. */
    MedianFinder() {
    }
    
    void addNum(int num) {
        if (nums.empty())
            nums.push_back(num);
        else
        {
            int i = 0;
            for (; i < nums.size(); ++i)
                if (nums[i] > num)
                    break;
            nums.insert(nums.begin() + i, num);            
        }
    }
    
    double findMedian() {
        int n = nums.size();
        if (n & 0x1)
            return nums[n / 2];
        else
            return (double)(nums[n/2] + nums[n/2-1]) / 2;
    }
};

```



* **方法二**

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

