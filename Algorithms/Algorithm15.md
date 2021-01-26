- [十五.细节实现专题](#十五细节实现专题)
        - [7.整数反转](#7整数反转)
        - [9.回文数](#9回文数)
        - [56.合并区间](#56合并区间)
        - [57.插入区间](#57插入区间)
        - [14.最长公共前缀](#14最长公共前缀)
        - [5.最长回文子串](#5最长回文子串)
        - [3.无重复字符的最长子串](#3无重复字符的最长子串)
        - [128.最长连续序列](#128最长连续序列)
        - [76.最小覆盖子串](#76最小覆盖子串)
        - [30.串联所有单词的子串](#30串联所有单词的子串)
        - [54.螺旋矩阵](#54螺旋矩阵)
        - [59.螺旋矩阵2](#59螺旋矩阵2)
        - [6.Z字形变换](#6z字形变换)
        - [29.两数相除](#29两数相除)
        - [191. 位1的个数](#191-位1的个数)
        - [50.Pow(x,n)](#50powxn)
        - [68.文本左右对齐](#68文本左右对齐)
        - [剑指 Offer 61. 扑克牌中的顺子](#剑指-offer-61-扑克牌中的顺子)


# 十五.细节实现专题

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


-----------------------------
##### 14.最长公共前缀
>题目描述：编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串 ""。
说明:
所有输入只包含小写字母 a-z 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-common-prefix

解题思路：由于字符串数组也是一个M行N列的数组，那么直接按照列进行遍历即可，平均情况是O(k*M),k为前缀长度，最差的情况是O(M*N)。

时间复杂度：O(M)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty())
            return string();
        int j = 0;
        for (; j < strs[0].size(); j++)
            for (int i = 0; i < strs.size(); i++)
                if (strs[i][j] != strs[0][j])
                    return strs[0].substr(0, j);
        return strs[0];
    }
};
```

<br>

-----------------------------
##### 5.最长回文子串
>题目描述：给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-palindromic-substring

* **解法一**

解题思路：暴力法，用两层for循环O(N^2)，再使用一层for循环判断是否为回文。

时间复杂度：O(N^3)

空间复杂度：O(1)

* **解法二**

解题思路：动态规划，dp[i][j],i代表起始位置，j代表结束位置。二维动态数组只需要填充右上部分，并且是从左往右一列一列的填充。
状态转移方程：dp[i][j] = (s[i]==s[j] && dp[i+1][j-1])

时间复杂度：O(N^2)

空间复杂度：O(N^2)

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int maxLen = 0;
        pair<int,int> res;
        int N = s.size();
        vector<vector<int>> dp(N, vector<int>(N, 0));
        for (int j = 0; j < N; j++)
        {
            for (int i = 0; i <= j; i++)
            {
                if (i == j)
                    dp[i][j] = 1;
                else if(j-i==1 || j-i==2)
                    dp[i][j] = (s[i]==s[j]);
                else
                    dp[i][j] = (dp[i+1][j-1]&&s[i]==s[j]);
                
                if (dp[i][j] && maxLen<j-i+1)
                {
                    maxLen = j-i+1;
                    res.first = i, res.second = j;
                }
            }
        } 
        int i = res.first, j = res.second;
        return s.substr(i, j-i+1);
    }
};

```


* **解法三**

解题思路：中心扩展法，以一个for循环遍历元素，该元素作为回文子串的中心左右两边扩散。注意需要区分最长回文的子串有奇数个还是偶数个。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.empty())
            return string();
        string ans;
        int maxLen = 0;
        for (int i = 0; i < s.size(); i++)
        {
            //回文串字符数为奇数
            int l = i, r = i;
            while (0<=l&&r<s.size() && s[l]==s[r])
                l--, r++;
            if (r-l-1 > maxLen)
            {
                maxLen = r - l - 1;
                ans = s.substr(l+1, maxLen);
            }

            //回文串字符数为偶数
            l = i, r = i+1;
            while (0<=l&&r<s.size() && s[l]==s[r])
                l--, r++;
            if (r-l-1 > maxLen)
            {
                maxLen = r - l - 1;
                ans = s.substr(l+1, maxLen);
            }
        }
        return ans;
    }
};

```


<br>



---------------------------
##### 3.无重复字符的最长子串
>题目描述:给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。
s 由英文字母、数字、符号和空格组成

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters

解题思路：滑动窗口法，用双指针模拟deque，创建辅助哈希set，双指针指向的范围存放的便是子串，进行一次遍历即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if (s.empty())
            return 0;
        int ans = 0;
        int i = 0, j = 0;
        unordered_set<char> hashset;
        while(j < s.size())
        {
            if (hashset.count(s[j]))
            {
                while (hashset.count(s[j]))
                {
                    hashset.erase(s[i]);
                    i++;
                }
            }
            hashset.insert(s[j]);
            ans = std::max(ans, j-i+1);
            j++;
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


解题思路：并查集，下标代表该数字，元素代表从该数字出发能够到达的位置；

时间复杂度：O(N)

空间复杂度：O(N)


```cpp
class Solution {
    unordered_map<int,int> union_find; //并查集
public:
    int find(int x)
    {
        if (union_find.count(x))
        {
            union_find[x] = find(union_find[x]);
            return union_find[x];
        }
        else
            return x;
    }

    int longestConsecutive(vector<int>& nums) {
        for (auto& e: nums)
            union_find[e-1] = e;
        int ans = 0;
        for (auto& e: nums)
        {
            union_find[e-1] = find(e);
            ans = std::max(ans, union_find[e-1]-(e-1));
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

解题思路：创建一个vector<string>数组存储Z字形变换，然后按照行进行读取即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows == 1)
            return s;
        vector<string> vec(numRows);
        int i = 0, add = 1;
        for (auto& e: s)
        {
            vec[i].push_back(e);
            i += add;
            if (i == numRows-1 || i==0)
                add = -add;
        }
        string ans;
        for (auto& e: vec)
            ans += e;
        return ans;
    }
};

```

<br>

---------------------------
##### 29.两数相除
>题目描述:给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。
返回被除数 dividend 除以除数 divisor 得到的商。
整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及 truncate(-2.7335) = -2
被除数和除数均为 32 位有符号整数。
除数不为 0。
假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−2^31,  2^31 − 1]。本题中，如果除法结果溢出，则返回 2^31 − 1。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/divide-two-integers

解题思路：既然不能够使用乘除法和mod法，那么就使用不断相减的策略，计算减了多少次即可，需要用快速幂运算进行优化，其中需要注意dividend , divisor的正负号。

时间复杂度：O(logx)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        long a = dividend>=0 ? dividend : -(long)dividend;
        long b = divisor>=0 ? divisor : -(long)divisor;
        long ans = 0;
        while (a >= b)
        {
            int i = 0;
            while (a >= (b<<i))
            {
                a -= (b<<i);
                ans += (1<<i);
                i++;
            }
        } 
        ans =  ((dividend>>31)^(divisor>>31)) ? -ans : ans;
        return ans>INT32_MAX ? INT32_MAX : ans;
    }
};

```

<br>



---------------------------
##### 191. 位1的个数
>题目描述:编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为汉明重量）。
提示：
请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
在 Java 中，编译器使用二进制补码记法来表示有符号整数。因此，在上面的 示例 3 中，输入表示有符号整数 -3。
输入必须是长度为 32 的 二进制串 。
进阶：
如果多次调用这个函数，你将如何优化你的算法？
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/number-of-1-bits

解题思路：可以用二进制右移不断记录二进制末尾的1即可，这样的时间复杂度为O(logX)；还可以进行优化，我们知道 n-1 可以将 n最右边的 1 变为 0111.. 这样的格式，也就是令其减少一个二进制位1，那么再 n&(n-1) 即可得到消去一个 1 之后的数，假设该数在二进制位中有k个1，那么时间复杂度为 O(k)

时间复杂度：O(k)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ans = 0;
        while (n)
        {
            n = n&(n-1);
            ans ++;
        }
        return ans;
    }
};
```

<br>


---------------------------
##### 50.Pow(x,n)
>题目描述:实现 pow(x, n) ，即计算 x 的 n 次幂函数。
-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−2^31, 2^31 − 1] 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/powx-n

解题思路：将n看做二进制进行不断往右移位操作，当二进制n最右边为1时，说明此时可以进行累乘。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    double Pow(double x, long n)
    {
        double ans = 1;
        while (n)
        {
            if (n & 0x1)
                ans *= x;
            n >>= 1;
            x *= x;
        }
        return ans;
    }

    double myPow(double x, int n) {
        if (0 == n)
            return 1;
        if (1 == n)
            return x;
        if (0 == x)
            return 0;
        if (1 == x)
            return 1;

        if (n < 0)
            return 1.0 / Pow(x, -(long)n);
        else
            return Pow(x, n);
    }
};

```

<br>


---------------------------
##### 68.文本左右对齐
>题目描述:给定一个单词数组和一个长度 maxWidth，重新排版单词，使其成为每行恰好有 maxWidth 个字符，且左右两端对齐的文本。
你应该使用“贪心算法”来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格 ' ' 填充，使得每行恰好有 maxWidth 个字符。
要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。
文本的最后一行应为左对齐，且单词之间不插入额外的空格。
说明:
单词是指由非空格字符组成的字符序列。
每个单词的长度大于 0，小于等于 maxWidth。
输入单词数组 words 至少包含一个单词。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/text-justification

解题思路：

时间复杂度：

空间复杂度：

```cpp

```

<br>

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


