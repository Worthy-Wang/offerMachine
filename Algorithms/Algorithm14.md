- [十四.细节实现专题](#十四细节实现专题)
        - [7.整数反转](#7整数反转)
        - [9.回文数](#9回文数)
        - [56.合并区间](#56合并区间)
        - [57.插入区间](#57插入区间)
        - [76.最小覆盖子串](#76最小覆盖子串)
        - [43.字符串相乘](#43字符串相乘)
        - [30.串联所有单词的子串](#30串联所有单词的子串)
        - [118.杨辉三角](#118杨辉三角)
        - [119.杨辉三角2](#119杨辉三角2)
        - [54.螺旋矩阵](#54螺旋矩阵)
        - [59.螺旋矩阵2](#59螺旋矩阵2)
        - [6.Z字形变换](#6z字形变换)
        - [29.两数相除](#29两数相除)
        - [68.文本左右对齐](#68文本左右对齐)
        - [149.直线上最多的点数](#149直线上最多的点数)


# 十四.细节实现专题

---------------------------
##### 7.整数反转
>题目描述:给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-integer

解题思路：不断对x%10 再/10，将结果累加，并判断有没有越界。

时间复杂度：O(logX)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int reverse(int x) {
        long long ans = 0;
        while (x)
        {
            ans = ans * 10 + x%10;
            x /= 10;
            if (ans > INT32_MAX)
                return 0;
            if (ans < INT32_MIN)
                return 0;
        }
        return ans;
    }
};

```

<br>


---------------------------
##### 9.回文数
>题目描述:判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
你能不将整数转为字符串来解决这个问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-number

解题思路：该题可以用上一题的解法，将数字进行反转再判断是否相同；注意负数不是回文数。

时间复杂度：O(logX)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0)
            return false;
        long res = 0, old = x;
        while (x)
        {
            res = 10 * res + x % 10;
            x /= 10;
        }
        return res==old;
    }
};

```

<br>


---------------------------
##### 56.合并区间
>题目描述:给出一个区间的集合，请合并所有重叠的区间。
intervals[i][0] <= intervals[i][1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-intervals

解题思路：直接遍历集合，比较ans.back() 是否与 遍历的元素相连，若相连接的话 修改， 没有连接的话则加入。

时间复杂度：O(NlogN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> ans;
        ans.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); i++)
        {
            if (ans.back()[1] >= intervals[i][0]) //连在一起
            {
                if (intervals[i][1] > ans.back()[1])
                    ans.back()[1] = intervals[i][1];
            }else //没有连在一起
                ans.push_back(intervals[i]);
        }
        return ans;
    }
};

```

<br>



---------------------------
##### 57.插入区间
>题目描述:给出一个无重叠的 ，按照区间起始端点排序的区间列表。
在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/insert-interval

解题思路：直接适用上一题的方法即可，并且可以不用排序，直接找到位置插入即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int i = 0;
        for (; i < intervals.size(); i++)
            if (intervals[i][0] >= newInterval[0])
                break;
        intervals.insert(intervals.begin()+i, newInterval);

        vector<vector<int>> ans;
        ans.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); i++)
        {
            if (ans.back()[1] >= intervals[i][0]) //连在一起
            {
                if (intervals[i][1] > ans.back()[1])
                    ans.back()[1] = intervals[i][1];
            }else //没有连在一起
                ans.push_back(intervals[i]);
        }
        return ans;        
    }
};

```

<br>



---------------------------
##### 76.最小覆盖子串
>题目描述:给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。
注意：如果 s 中存在这样的子串，我们保证它是唯一的答案。
s 和 t 由英文字母组成
进阶：你能设计一个在 o(n) 时间内解决此问题的算法吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-window-substring

解题思路： 先想到暴力法，两次 for循环，比较后找到最小的子串，需要用hashset来存储t串中的元素。
再想到用滑动窗口的优化法，设置双指针i j ，当滑动窗口中包含t串中的所有元素时，i++； 没有包含t串中的所有元素时，j++。

时间复杂度：O(N) N为s串的长度

空间复杂度：O(M) M为t串的长度

```cpp
17:46


```

<br>




---------------------------
##### 43.字符串相乘
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>




---------------------------
##### 30.串联所有单词的子串
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp
17:30

```

<br>


---------------------------
##### 118.杨辉三角
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>




---------------------------
##### 119.杨辉三角2
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>




---------------------------
##### 54.螺旋矩阵
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>




---------------------------
##### 59.螺旋矩阵2
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>




---------------------------
##### 6.Z字形变换
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>




---------------------------
##### 29.两数相除
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>




---------------------------
##### 68.文本左右对齐
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>




---------------------------
##### 149.直线上最多的点数
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>


