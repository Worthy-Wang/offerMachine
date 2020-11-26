


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

解题思路：

时间复杂度：

空间复杂度：

17:11
```cpp


```


<br>

------------------------------
1.两数之和
15.三数之和
16.最接近的三数之和
18.四数之和
27.移除元素
31.下一个排列
60.排列序列
36.有效的数独
42.接雨水
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



