- [十四.细节实现专题](#十四细节实现专题)
        - [7.整数反转](#7整数反转)
        - [9.回文数](#9回文数)
        - [56.合并区间](#56合并区间)
        - [57.插入区间](#57插入区间)
        - [76.最小覆盖子串](#76最小覆盖子串)
        - [415.字符串相加](#415字符串相加)
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

解题思路： 先想到暴力法，两次 for循环，比较后找到最小的子串，需要用hashmap来存储t串中的元素。
再想到用滑动窗口的优化法，设置双指针i j ，当滑动窗口中包含t串中的所有元素时，i++； 没有包含t串中的所有元素时，j++。

时间复杂度：O(N) N为s串的长度

空间复杂度：O(M) M为t串的长度

```cpp
class Solution {
public:
 
bool check(unordered_map<char, int> &Smap, unordered_map<char, int> &Tmap)
{
    for (auto &e : Tmap)
    {
        if (Smap[e.first] < e.second)
            return false;
    }
    return true;
}

string minWindow(string s, string t)
{
    unordered_map<char, int> Smap, Tmap;
    for (auto &e : t)
        Tmap[e]++;
    int i = 0, j = -1, n = s.size(), minI = 0, minJ = -1, minLen = INT32_MAX;
    while (j < n)
    {
        if (check(Smap, Tmap)) //滑动窗口已经覆盖子串t , i++
        {
            while (!Tmap.count(s[i]))
                i++;
            if (j - i + 1 < minLen)
            {
                minLen = j - i + 1;
                minI=  i;
                minJ = j;
            }
            Smap[s[i]]--;
            i++;
        }
        else //滑动窗口未覆盖子串t, j++
        {
            j++;
            if (Tmap.count(s[j]))
                Smap[s[j]]++;
        }
    }
    return s.substr(minI, minJ - minI + 1);
}

};

```

<br>

---------------------------
##### 415.字符串相加
>题目描述:给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。
提示：
num1 和num2 的长度都小于 5100
num1 和num2 都只包含数字 0-9
num1 和num2 都不包含任何前导零
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-strings

解题思路：设置进位carry,从后往前逐位进行累加即可

时间复杂度：O(max(M,N))

空间复杂度：O(1)

```cpp
class Solution {
public:
    string addStrings(string num1, string num2) {
        int i = num1.size()-1, j = num2.size()-1, carry = 0;
        string ans;
        while (i>=0 || j>=0 || carry)
        {
            int x = i>=0 ? num1[i--]-'0' : 0;
            int y = j>=0 ? num2[j--]-'0' : 0;
            int sum = x + y + carry;
            ans = to_string(sum%10) + ans;
            carry = sum/10;
        }
        return ans;
    }
};
```

<br>


---------------------------
##### 43.字符串相乘
>题目描述:给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。
num1 和 num2 的长度小于110。
num1 和 num2 只包含数字 0-9。
num1 和 num2 均不以零开头，除非是数字 0 本身。
不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/multiply-strings

解题思路：两个for循环进行累乘，将结果进行累加

时间复杂度：O(M*N)

空间复杂度：O(M+N)

```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        if ('0'==num1[0] || '0'==num2[0])
            return "0";
        reverse(num1.begin(), num1.end());
        reverse(num2.begin(), num2.end());
        vector<int> temp(num1.size() + num2.size(), 0);
        for (int j = 0; j < num2.size(); j++)
        {
            for (int i = 0; i < num1.size(); i++)
            {
                int multi = (num1[i]-'0') * (num2[j]-'0');
                multi += temp[i+j];  //还得加上原来已有的
                temp[i+j] = multi%10;
                temp[i+j+1] += multi / 10;
            }
        }
        
        string ans;
        int i = temp.size()-1;
        while (0 == temp[i])  //跳过前面的0
            i--;
        while (i >= 0)
        {
            ans.push_back(temp[i] + '0');
            i--;
        }
        return ans;
    }
};
```

<br>




---------------------------
##### 30.串联所有单词的子串
>题目描述:给定一个字符串 s 和一些长度相同的单词 words。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。
注意子串要与 words 中的单词完全匹配，中间不能有其他字符，但不需要考虑 words 中单词串联的顺序。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words

解题思路：暴力法，将words看做一个字符串进行比较即可

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
public:
    bool check(string s, unordered_map<string,int>& wordMap, int n, int m)
    {
        unordered_map<string,int> hashmap;
        for (int i = 0; i < s.size(); i+=m)
            hashmap[s.substr(i, m)]++;
        return hashmap==wordMap;
    }

    vector<int> findSubstring(string s, vector<string>& words) {
        unordered_map<string, int> wordMap;
        for (auto& e: words)
            wordMap[e]++;
        int n = words.size(), m = words[0].size(), wordsLen = m*n;
        vector<int> ans;

        for (int i = 0; i < s.size()-wordsLen +1; i++)
        {
            string s2 = s.substr(i, wordsLen);
            if (check(s2, wordMap, n, m))
                ans.push_back(i);
        }
        return ans;
    }
};

```

<br>


---------------------------
##### 118.杨辉三角
>题目描述:给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。
在杨辉三角中，每个数是它左上方和右上方的数的和。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pascals-triangle

解题思路：杨辉三角的本质是动态规划，直接在for循环中创建数组再加入即可。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;
        for (int i = 0; i < numRows; i++)
        {
            vector<int> vec(i+1,  1);
            for (int j = 1; j < i; j++)
                vec[j] = ans[i-1][j] + ans[i-1][j-1];
            ans.push_back(std::move(vec));
        }
        return ans;
    }
};
```

<br>




---------------------------
##### 119.杨辉三角2
>题目描述:给定一个非负索引 k，其中 k ≤ 33，返回杨辉三角的第 k 行。
在杨辉三角中，每个数是它左上方和右上方的数的和。
进阶：
你可以优化你的算法到 O(k) 空间复杂度吗？
注意此题有一个坑，那就是k是从0开始取的，在程序中我们先将k+1即可。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pascals-triangle-ii

解题思路：动态规划优化法，用两个交替的数组即可。

时间复杂度：O(K^2)

空间复杂度：O(K)

```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        rowIndex++;
        vector<int> dp0(rowIndex, 1);
        for (int i = 0; i < rowIndex; i++)
        {
            vector<int> dp1(rowIndex, 1);
            for (int j = 1; j < i; j ++)
                dp1[j] = dp0[j-1] + dp0[j];
            dp0 = dp1;
        }
        return dp0;
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
##### 6.Z字形变换
>题目描述:将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
请你实现这个将字符串进行指定行数变换的函数：
string convert(string s, int numRows);

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zigzag-conversion

解题思路：

时间复杂度：

空间复杂度：

```cpp
19：11

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


