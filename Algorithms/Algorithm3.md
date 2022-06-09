## 目录
- [三.字符串专题](#三字符串专题)
    - [8.字符串转换整数](#8字符串转换整数)
    - [剑指 Offer 20. 表示数值的字符串](#剑指-offer-20-表示数值的字符串)
    - [67.二进制求和](#67二进制求和)
    - [415.字符串相加](#415字符串相加)
    - [43.字符串相乘](#43字符串相乘)
    - [5.最长回文子串](#5最长回文子串)
    - [125.验证回文串](#125验证回文串)
    - [14.最长公共前缀](#14最长公共前缀)
    - [3.无重复字符的最长子串](#3无重复字符的最长子串)
    - [76.最小覆盖子串](#76最小覆盖子串)
    - [30.串联所有单词的子串](#30串联所有单词的子串)
    - [6.Z字形变换](#6z字形变换)
    - [151. 翻转字符串里的单词](#151-翻转字符串里的单词)
    - [剑指 Offer 05. 替换空格](#剑指-offer-05-替换空格)
    - [剑指 Offer 58 - II. 左旋转字符串](#剑指-offer-58---ii-左旋转字符串)
    - [28.实现strStr()](#28实现strstr)
    - [49.字母异位词分组](#49字母异位词分组)
    - [71.简化路径](#71简化路径)
    - [58.最后一个单词的长度](#58最后一个单词的长度)


### 三.字符串专题



-----------------------------
##### 8.字符串转换整数
>题目描述：请你来实现一个 atoi 函数，使其能将字符串转换成整数。
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：
如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0 。
提示：
本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2^31,  2^31 − 1]。如果数值超过这个范围，请返回  INT_MAX (2^31 − 1) 或 INT_MIN (−2^31) 。
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/string-to-integer-atoi

解题思路：经典的atoi，stoi 题，也就是将字符串转换为整数。字符串的前缀可以为空格，有效整数前可以用+-号区分，有效整数后的字符可以直接忽略，若转换失败返回 0，另外需要注意转换的整数不能超过 INT32_MIN 和 INT32_MIN 。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int myAtoi(string s) {
        long sum = 0;
        int i = 0;
        int flag = 1;
        while (isspace(s[i])) i++; //忽略前缀空格
        if ('+'==s[i])
            i++;
        else if ('-' == s[i])
        {
            i++;
            flag = -1;
        }
        while (isdigit(s[i]))
        {
            sum = sum * 10 + s[i] - '0';
            if (sum*flag > INT32_MAX)
                return INT32_MAX;
            else if (sum*flag < INT32_MIN)
                return INT32_MIN;
            i++;
        }
        return sum * flag;
    }
};

```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 剑指 Offer 20. 表示数值的字符串
>题目描述:请实现一个函数用来判断字符串是否表示数值（包括整数和小数），也就是验证给定的字符串是否可以解释为十进制数字。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"-1E-16"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof

解题思路：先写一个最难的   -3.12e-3    .前后有数字即可，e可存在可不存在，若e存在后面就必须接上数字。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isNumber(string s) {
        int i = 0;
        while (isspace(s[i])) i++; //跳过空格
        if ('+'==s[i] || '-'==s[i]) i++; //跳过+-号
        bool ans1 = false, ans2 = false;
        while(isdigit(s[i]))
        {
            i++;
            ans1 = true;
        }
        if ('.' == s[i]) i++; //跳过.
        while(isdigit(s[i]))
        {
            i++;
            ans2 = true;
        }
        if (!ans1 && !ans2) //如果小数点前后都没有数字，返回false
            return false;

        if ('e'==s[i] || 'E'==s[i])//如果数字后面有e的情况
        {
            i++;
            if ('+'==s[i] || '-'==s[i])
                i++;
            bool ans3 = false;
            while(isdigit(s[i]))
            {
                i++;
                ans3 = true;
            }
            if (!ans3)
                return false;
        }        

        //有效数字遍历完成，往后遍历空格
        while (i != s.size())
        {
            if (!isspace(s[i]))
                return false;
            i++;
        }

        return true;
    }
};
```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


-----------------------------
##### 67.二进制求和
>题目描述：给你两个二进制字符串，返回它们的和（用二进制表示）。
输入为 非空 字符串且只包含数字 1 和 0。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-binary

解题思路：先遍历一遍两个字符串，将较短的字符串用'0'在前部进行填充，竖向的两两相加，即可得到结果。

时间复杂度：O(max(M, N))

空间复杂度：O(1)

```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        int m = a.size(), n = b.size();
        string ans;
        int carry = 0;
        int i = m-1, j = n-1;
        while (i >= 0 || j >= 0 || carry)
        {
            int num1 = i >= 0 ? a[i--] - '0' : 0;
            int num2 = j >= 0? b[j--] - '0' : 0;
            int sum = num1 + num2 + carry;
            int num = sum % 2;
            carry = sum / 2;
            ans = to_string(num) + ans;
        }
        return ans;
    }
};
```

<br>



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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
        if ("0" == num1 || "0" == num2)
            return "0";
        int m = num1.size(), n = num2.size();
        vector<int> ans(m + n, 0);
        for (int i = m - 1; i >= 0; --i)
            for (int j = n -1; j >= 0; --j)
            {
                int multi = (num1[i] - '0') * (num2[j] - '0');
                ans[i + j + 1] += multi;
            }
        for (int i = ans.size() - 1; i > 0; --i)
        {
            ans[i-1] += ans[i] / 10;
            ans[i] %= 10;
        }

        int i = 0;
        string s;
        while (0 == ans[i])
            ++i;
        while (i < ans.size())
        {
            s = s + to_string(ans[i]);
            ++i;
        }
        return s;
    }
};
```

<br>



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        int maxLen = 0;
        string ans = "";
        for (int j = 0; j < n; ++j)
            for (int i = 0; i <= j; ++i)
            {
                if (i == j)
                    dp[i][j] = 1;
                else if (j == i + 1)
                    dp[i][j] = (s[i] == s[j]);
                else
                    dp[i][j] = (s[i] == s[j] && dp[i+1][j-1]);

                if (dp[i][j])
                {
                    int len = j - i + 1;
                    if (len > maxLen)
                    {
                        maxLen = len;
                        ans = s.substr(i, len);
                    }
                }
            }
        return ans;
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



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


-----------------------------
##### 125.验证回文串
>题目描述：给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。
说明：本题中，我们将空字符串定义为有效的回文串。
示例 1:
输入: "A man, a plan, a canal: Panama"
输出: true

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-palindrome

解题思路：双指针法不断向中间靠拢即可。

>此处需要使用一些C的函数，如
**isdigit** :判断是否为数字
**isalpha** : 判断是否为字母，大小写均可
**isalnum** : 判断是否为字母或数字
**isupper** : 判断字母是否为大写
**islower** : 判断字母是否为小写
**isspace** : 是否为空格
**isblank** : 是否为空白
**tolower** : 将大写字母转为小写
**toupper** : 将小写字母转为大写

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        if (s.empty())
            return true;
        int i = 0, j = s.size()-1;
        while (i < j)
        {
            while (i < j && !isalnum(s[i]))
                i++;
            while (i < j && !isalnum(s[j]))
                j--;
            if (i < j && tolower(s[i++])!=tolower(s[j--]))
                return false;
        }
        return true;
    }
};

```

<br>



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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
        for (int j = 0; j < strs[0].size(); ++j)
            for (int i = 0; i < strs.size(); ++i)
                if (strs[0][j]!=strs[i][j] || j>=strs[i].size())
                    return strs[0].substr(0, j);
        return strs[0];
    }
};
```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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
        int l = 0, r = 0;
        int ans = 0;
        unordered_set<char> hashset;
        while (r < s.size())
        {
            while (hashset.find(s[r]) != hashset.end())
            {
                hashset.erase(s[l]);
                ++l;
            }   
            hashset.insert((s[r]));
            ans = std::max(ans, r - l + 1);
            ++r;         
        }
        return ans;
    }
};
```

<br>




<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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
    bool check(unordered_map<char,int>& sMap, unordered_map<char,int>& tMap)
    {
        for (const auto& e: tMap)
            if (sMap[e.first] < e.second)
                return false;
        return true;
    }

    string minWindow(string s, string t) {
        unordered_map<char,int> sMap, tMap;
        for (const auto& ch: t)
            tMap[ch]++;
        int l = 0, r = 0, minLen = INT32_MAX;
        string ans;
        while (r < s.size())
        {
            sMap[s[r]]++;
            while (check(sMap, tMap))
            {
                if (r - l + 1 < minLen)
                {
                    minLen = r - l + 1;
                    ans = s.substr(l, minLen);
                }
                sMap[s[l]]--;
                ++l;
            }
            ++r;
        }
        return ans;
    }
};
```

<br>



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 30.串联所有单词的子串
>题目描述:给定一个字符串 s 和一些**长度相同**的单词 words。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。
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




<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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
        if (1 == numRows)
            return s;
        int idx = 0, i = 0, add = 1;
        vector<string> nums(numRows);
        while (idx < s.size())
        {
            nums[i].push_back(s[idx]);
            if (0 == i)
                add = 1;
            else if (numRows-1 == i)
                add = -1;
            i += add;
            ++idx;
        }
        
        string ans;
        for (auto& s: nums)
            ans += s;
        return ans;
    }
};

```

<br>





<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 151. 翻转字符串里的单词
给定一个字符串，逐个翻转字符串中的每个单词。
无空格字符构成一个 单词 。
输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
进阶：
请尝试使用 O(1) 额外空间复杂度的原地解法。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-words-in-a-string

* **解法一**

解题思路：stringstream 作为辅助

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    string reverseWords(string s) {
        stringstream ss(s);
        string res, temp;
        while (ss >> temp)
            res = temp + " " + res;
        return res.substr(0, res.size()-1);
    }
};

```


* **解法二**

解题思路：循环遍历该字符串

时间复杂度：O(N)

空间复杂度：O(1) 

```cpp
class Solution {
public:
    string reverseWords(string s) {
        string ans;
        int i = 0;
        while (i < s.size())
        {
            if (isspace(s[i]))
                ++i;
            else
            {
                int j = i+1;
                while (j < s.size() && !isspace(s[j]))
                    ++j;
                ans = s.substr(i, j-i) + " " + ans;
                i = j;
            }            
        }
        ans.pop_back();
        return ans;
    }
};

```


<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 剑指 Offer 05. 替换空格
>题目描述:请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof


解题思路：假设不能够在原字符串s上面修改，那么创建新的串返回，这很简单；如果面试官要求必须在原字符串s上面进行修改，那么先统计出空格的数量（如果原本s的长度够的话就不需要扩容），再对s进行扩容，之后同时从尾部用双指针逆向返回。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string replaceSpace(string s) {
        int cnt = 0;
        for (auto& e: s)
            if (' ' == e)   
                cnt++;
        int n1 = s.size(), n2 = s.size() + cnt*2;
        s.resize(n2);
        int i = n1-1, j = n2-1;
        while (i>=0)
        {
            if (s[i] == ' ')
            {
                s[j--] = '0';
                s[j--] = '2';
                s[j--] = '%';
                i--;
            }else
                s[j--] = s[i--];
        }
        return s;
    }
};
```

当然，也可以不用在原地修改。

```cpp
class Solution {
public:
    string replaceSpace(string s) {
        string ans;
        for (const auto& e: s)
        {
            if (' ' == e)
                ans += "%20";
            else
                ans.push_back(e);
        }
        return ans;
    }
};
```


<br>

<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 剑指 Offer 58 - II. 左旋转字符串
>题目描述:字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof

解题思路：直接使用字符串的拼接即可

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        if (n<=0 || n>s.size())
            return s;
        return s.substr(n, s.size()-n) + s.substr(0, n);
    }
};
```

<br>



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


-----------------------------
##### 28.实现strStr()
>题目描述：实现 strStr() 函数。
给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。
示例 1:
输入: haystack = "hello", needle = "ll"
输出: 2
当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。
对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/implement-strstr

* **解法一**

解题思路：BF暴力法，从每一个haystack的元素开始遍历，往后看是否满足 needle字符串。该方法超时！

时间复杂度：O(N*M)

空间复杂度：O(1)


* **解法二**

解题思路：RK算法，首先对needle进行哈希求值并求出长度n，再对haystack进行一次遍历并在haystack中不断计算n长度的哈希值，若haystakc和needle的哈希值相等时，需要进行一遍对比（因为可能会出现哈希冲突的情况），如果仍然相等则说明两个字符串匹配。
RK算法的缺点就是如果哈希冲突太多的话，就会退化成BF算法，这个也很好理解。


时间复杂度：O(M+N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int sum1 = 0, sum2 = 0, len1 = haystack.size(), len2 = needle.size();
        for (auto& e: needle)
            sum2 += e;
        int start = 0, end = len2-1;
        for (int i = start; i <= end; i++)
            sum1 += haystack[i];
        while (end < len1)
        {
            if (sum1 == sum2)
            {
                bool flag = true;//预防哈希冲突
                for (int i = 0; i < len2; i++)
                    if (haystack[i+start] != needle[i])
                    {
                        flag = false;
                        break;
                    }
                if (flag)
                   return start;
            }
            end++;
            start++;
            if (end < len1)
                sum1 = sum1 + haystack[end]-haystack[start-1];
        }
        return -1;
    }
};

```

* **解法三**

解题思路：KMP算法，在haystack设置永不回退的指针i，在needle上设置动态回退的指针j，其中需要辅助数组next，每一次匹配失败时，**j = next[j]** ，也就是回退到之前已经拥有重复前缀的位置。
next数组其实也很好理解，需要用到动态规划的思想。它根据 前缀的前一部分 和 前缀的后一部分 是否相等，用来记录匹配失败时j指针需要回退的位置。因为如果 前缀的后一部分 能够匹配成功，那么 前缀的后一部分 也能够匹配成功，也就无需再一次让 j 指针从头开始，这就是next 数组的原理。

时间复杂度：O(M+N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    void getNext(string& needle, vector<int>& next)
    {
        int i = 0, j = -1;
        next[0] = -1;
        while (i < needle.size())
        {
            if (-1==j || needle[i]==needle[j])
            {
                ++i, ++j;
                next[i] = j;
            }else
                j = next[j];
        }
    }

    int strStr(string haystack, string needle) {
        int n1 = haystack.size(), n2 = needle.size();
        if (0 == n2)
            return 0;
        vector<int> next(n2 + 1);
        getNext(needle, next);
        int i = 0, j = 0;
        while (i < n1)
        {
            if (-1==j || haystack[i]==needle[j])
            {
                ++i, ++j;
                if (j == n2)
                    return i-n2;
            }else
                j = next[j];
        }
        return -1;
    }
};

```

<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


-----------------------------
##### 49.字母异位词分组
>题目描述：给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。
示例:
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
说明：
所有输入均为小写字母。
不考虑答案输出的顺序。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/group-anagrams

解题思路：设置一个哈希表 key 为 每个字符串排序后的串，value 为 符合该key值的 字符串集合，进行一次遍历即可。

时间复杂度：O(N*K*logK) k为字符串的长度，N为字符串数组的长度

空间复杂度：O(N) 

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> hashmap;
        string key;
        for(auto& s: strs)
        {
            key = s;
            sort(key.begin(), key.end(), std::less<char>());
            hashmap[key].push_back(s);
        }
        vector<vector<string>> ans;
        for(auto& e: hashmap)
            ans.push_back(e.second);
        return ans;
    }
};

```

<br>





<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


-----------------------------
##### 71.简化路径
>题目描述：以 Unix 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。
在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。    
请注意，返回的规范路径必须始终以斜杠 / 开头，并且两个目录名之间必须只有一个斜杠 /。最后一个目录名（如果存在）不能以 / 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/simplify-path

解题思路：该题的核心就是通过一次遍历，将所有合法的文件名 file 存储起来，然后拼接成为返回string串即可，中间需要以 '/' 为间隔进行不断的跳过。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    string simplifyPath(string path) {
        if (path.empty())
            return string();
        vector<string> vec; //存放分割出来的文件名
        stringstream ss(path);
        string file;
        while (getline(ss, file, '/'))
        {
            if ("" == file)
                continue;
            else if ("." == file)
                continue;
            else if (".." == file)
            {
                if (!vec.empty()) 
                    vec.pop_back();
            }
            else 
                vec.push_back(file);
        }

        string ans;
        for(auto& e: vec)
            ans = ans + "/" + e;
        if (ans.empty())
            return "/";
        return ans;
    }
};


```

<br>

<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


-----------------------------
##### 58.最后一个单词的长度
>题目描述：给定一个仅包含大小写字母和空格 ' ' 的字符串 s，返回其最后一个单词的长度。如果字符串从左向右滚动显示，那么最后一个单词就是最后出现的单词。
如果不存在最后一个单词，请返回 0 。
说明：一个单词是指仅由字母组成、不包含任何空格字符的 最大子字符串。
示例:
输入: "Hello World"
输出: 5

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/length-of-last-word

* **解法一**

解题思路：直接从尾部开始遍历即可 

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
        int j = s.size()-1;
        while (0<=j && isspace(s[j]))
            j--;
        int i = j;
        while (0<=i && !isspace(s[i]))
            i--;
        return j-i;
    }
};

```


* **解法二**

解题思路：直接适用C++中的stringstream，用getline 或者 流操作 即可。

时间复杂度：O(N)

空间复杂度：O(1)



```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
        stringstream ss(s);
        string ans;
        while (ss >> ans);
        return ans.size();
    }
};
```

* **解法三**

解题思路：使用find_if 和  find_if_not

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
        reverse(s.begin(), s.end());
        auto beg = find_if_not(s.begin(), s.end(), [&](char ch){
            return ' ' == ch;
        });
        auto end = find_if(beg, s.end(), [&](char ch){
            return ' ' == ch;
        });
        return distance(beg, end);
    }
};

```


<br>


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>

