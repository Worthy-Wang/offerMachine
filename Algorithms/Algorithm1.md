


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



<br>

-----------------------
81.搜索旋转排序数组2
4.寻找两个正序数组的中位数
128.最长连续序列
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



