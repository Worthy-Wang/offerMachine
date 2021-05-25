- [十五.细节实现专题](#十五细节实现专题)
    - [66.加一](#66加一)
    - [7.整数反转](#7整数反转)
    - [9.回文数](#9回文数)
    - [56.合并区间](#56合并区间)
    - [57.插入区间](#57插入区间)
    - [14.最长公共前缀](#14最长公共前缀)
    - [3.无重复字符的最长子串](#3无重复字符的最长子串)
    - [76.最小覆盖子串](#76最小覆盖子串)
    - [30.串联所有单词的子串](#30串联所有单词的子串)
    - [128.最长连续序列](#128最长连续序列)
    - [6.Z字形变换](#6z字形变换)
    - [68.文本左右对齐](#68文本左右对齐)
    - [剑指 Offer 61. 扑克牌中的顺子](#剑指-offer-61-扑克牌中的顺子)
    - [149.直线上最多的点数](#149直线上最多的点数)
    - [93.复原IP地址](#93复原ip地址)
    - [12.整形转罗马数字](#12整形转罗马数字)
    - [13.罗马数字转整形](#13罗马数字转整形)
    - [17.电话号码的字母组合](#17电话号码的字母组合)


### 十五.细节实现专题



-----------------------------------
##### 66.加一
>题目描述：给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。
最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
你可以假设除了整数 0 之外，这个整数不会以零开头。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/plus-one

解题思路：该题从后往前一次遍历即可求得。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        if (digits.empty())
            return {};
        reverse(digits.begin(), digits.end());
        int carry = 1;
        for (int i = 0; i < digits.size(); ++i)
        {
            int sum = digits[i] + carry;
            carry = sum / 10;
            digits[i] = sum % 10;
        }
        if (carry)
            digits.push_back(1);
        reverse(digits.begin(), digits.end());
        return digits;
    }
};

```






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
        for (int i = 1; i < intervals.size(); ++i)  
        {
            if (intervals[i][0] > ans.back()[1])
                ans.push_back(intervals[i]);
            else if (intervals[i][1] <= ans.back()[1])
                continue;
            else
                ans.back()[1] = intervals[i][1];
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
        for (; i < intervals.size(); ++i)
            if (newInterval[0] <= intervals[i][0])
                break;
        intervals.insert(intervals.begin()+i, newInterval);
        vector<vector<int>> ans;
        ans.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); ++i)  
        {
            if (intervals[i][0] > ans.back()[1])
                ans.push_back(intervals[i]);
            else if (intervals[i][1] <= ans.back()[1])
                continue;
            else
                ans.back()[1] = intervals[i][1];
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
        for (int j = 0; j < strs[0].size(); ++j)
            for (int i = 0; i < strs.size(); ++i)
                if (strs[0][j]!=strs[i][j] || j>=strs[i].size())
                    return strs[0].substr(0, j);
        return strs[0];
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
        for (auto& e: tMap)
            if (e.second > sMap[e.first])
                return false;
        return true;
    }

    string minWindow(string s, string t)
    {
        unordered_map<char, int> sMap, tMap; //由于s和t中都可能有重复的字符，所以需要用哈希表表示
        for (auto &ch : t)
            tMap[ch]++;
        int i = 0, j = 0;
        int minLen = INT32_MAX, beg = 0, end = -1;
        while (j < s.size())
        {
            sMap[s[j]]++;
            if (check(sMap, tMap)) //滑动窗口已经覆盖t
            {
                while (check(sMap, tMap))
                {
                    if (minLen > (j - i + 1))
                    {
                        minLen = j - i + 1;
                        beg = i;
                        end = j;
                    }
                    sMap[s[i]]--;
                    ++i;
                }
            }
            //滑动窗口未覆盖t
            ++j;
        }
        return s.substr(beg, end - beg + 1);
    }
};
```

<br>



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





--------------------------
##### 128.最长连续序列
>题目描述：给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
要求：你可以设计并实现时间复杂度为 O(n) 的解决方案吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-consecutive-sequence


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



---------------------------
##### 149.直线上最多的点数
>题目描述:给定一个二维平面，平面上有 n 个点，求最多有多少个点在同一条直线上。
示例 1:
输入: [[1,1],[2,2],[3,3]]
输出: 3
解释:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
示例 2:
输入: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
输出: 4
解释:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/max-points-on-a-line

解题思路：依次遍历各个点，与后面的点进行计算斜率，找出同一个斜率中拥有的最多点数即可。需要注意两点：1.斜率可能出现与x轴或者y轴相平行，所以斜率的形式要用string表示；2.可能会出现重复的点的情况，所以用dup记录重复的点的个数。

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int GCD(int a, int b)
    {
        while (b)
        {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }

    int maxPoints(vector<vector<int>>& points) {
        int ans = 0;
        for (int i = 0; i < points.size(); ++i)
        {
            unordered_map<string, int> hashmap;//<斜率,点个数>
            int curMax = 0;
            int dup = 0;
            for (int j = i+1; j < points.size(); ++j)
            {
                int x1 = points[i][0], y1 = points[i][1];
                int x2 = points[j][0], y2 = points[j][1];
                int diffX = x2 - x1, diffY = y2 - y1, gcd = GCD(diffX, diffY);
                if (0==diffX && 0==diffY)
                {
                    dup++;
                    continue;
                }
                string key = to_string(diffX/gcd) + "/" + to_string(diffY/gcd);
                hashmap[key]++;
                curMax = std::max(curMax, hashmap[key]);
            }
            ans = std::max(curMax + 1 + dup, ans); //curMax+1是因为要加上i指向的这一个点, 加上dup是因为要加上重复的点
        }
        return ans;
    }
};

```

<br>


---------------------------
##### 93.复原IP地址
>题目描述:给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。
有效的 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。
例如："0.1.2.201" 和 "192.168.1.1" 是 有效的 IP 地址，但是 "0.011.255.245"、"192.168.1.312" j是 无效的 IP 地址。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/restore-ip-addresses

解题思路：DFS法，设置path辅助变量存储分割好的数字，设置start起始位置作为每一次数字的开头，剪枝操作实质上也就是将不符合规定的IP地址值跳过。

时间复杂度：O(N!)

空间复杂度：O(N)

```cpp
class Solution {
    vector<string> ans;
public:
    void DFS(string& s, int start, vector<string>& path)
    {
        if (start == s.size() && 4 == path.size())
        {
            string res = path[0] + "." + path[1] + "." + path[2] + "." + path[3];
            ans.push_back(std::move(res));
            return;
        }
        if (path.size() > 4)
            return;

        for (int i = start; i < s.size(); ++i)
        {
            string str = s.substr(start, i-start + 1);
            if (stoi(str) > 255)
                break;
            path.push_back(str);
            DFS(s, i+1, path);
            path.pop_back();
            if ("0" == str)
                break;
        }
    }

    vector<string> restoreIpAddresses(string s) {
        vector<string> path;
        DFS(s, 0, path);
        return ans;
    }
};



```

<br>



-----------------------------
##### 12.整形转罗马数字
>题目描述：罗马数字包含以下七种字符： I， V， X， L，C，D 和 M。 
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。
通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：
I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/integer-to-roman

解题思路：首先使用vector进行整形数字与罗马数字的映射（不用哈希表的原因是无法将数字进行从大到小的遍历），我们注意到 4, 9 这样的数是需要单独设置的，也就是这些需要有单独个性的数，需要添加到哈希表里面去；接下来再从最大的数字（1000）开始求余数，再将罗马数字累加即可得到结果。
   
时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string intToRoman(int num) {
        vector<pair<int, string>> vec{{1000,"M"}, {900,"CM"}, {500,"D"}, {400,"CD"}, {100,"C"}, {90,"XC"}, {50,"L"}, {40, "XL"}, {10,"X"}, {9, "IX"}, {5,"V"}, {4,"IV"}, {1,"I"}};
        string ans;
        for (auto& e: vec)
        {
            int cnt = num / e.first;
            for (int i = 0; i < cnt; i++)
                ans += e.second;
            num -= cnt * e.first;
        }
        return ans;
    }
};


```

<br>


-----------------------------
##### 13.罗马数字转整形
>题目描述：
罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。
通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：
I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/roman-to-integer

解题思路：创建哈希表实现 罗马数字->普通数字，从罗马数字的字符串开始遍历，注意罗马数字可能会是两个连续的字符。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<string, int> hashmap{{"I", 1}, {"IV", 4}, {"V",5}, {"IX", 9}, {"X", 10},  {"XL", 40}, {"L", 50}, {"XC", 90}, {"C", 100}, {"CD", 400}, {"D", 500}, {"CM", 900}, {"M", 1000}};
        int i = 0;
        int ans = 0;
        while (i < s.size())
        {
            if (hashmap.count(s.substr(i, 2)))
            {
                ans += hashmap[s.substr(i, 2)];
                i += 2;
            }else
            {
                ans += hashmap[s.substr(i, 1)];
                i += 1;
            }
        }
        return ans;
    }
};

<br>



---------------------------
##### 17.电话号码的字母组合
>题目描述:给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。此题需要看原链接。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number

解题思路：回溯法

时间复杂度：O(K^N) K是数字对应字符串的平均长度

空间复杂度：O(N) N是数字字符串的长度

```cpp
class Solution {
    vector<string> ans;
    unordered_map<char, string> hashmap{{'2',"abc"}, {'3',"def"},{'4',"ghi"},{'5',"jkl"},{'6',"mno"},{'7',"pqrs"},{'8',"tuv"}, {'9', "wxyz"}};
public:
    void DFS(string& digits, int start, string& path)
    {
        if (path.size() == digits.size())
        {
            ans.push_back(path);
            return;
        }

        string s = hashmap[digits[start]];
        for (int i = 0; i < s.size(); i++)
        {
            path.push_back(s[i]);
            DFS(digits, start+1, path);
            path.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        if (digits.empty())
            return {};
        string path;
        DFS(digits, 0, path);
        return ans;
    }
};

```

<br>


---------------------------
##### 17.电话号码的字母组合
>题目描述:给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。此题需要看原链接。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number

解题思路：回溯法

时间复杂度：O(K^N) K是数字对应字符串的平均长度

空间复杂度：O(N) N是数字字符串的长度

```cpp
class Solution {
    vector<string> ans;
    unordered_map<char, string> hashmap{{'2',"abc"}, {'3',"def"},{'4',"ghi"},{'5',"jkl"},{'6',"mno"},{'7',"pqrs"},{'8',"tuv"}, {'9', "wxyz"}};
public:
    void DFS(string& digits, int start, string& path)
    {
        if (path.size() == digits.size())
        {
            ans.push_back(path);
            return;
        }

        string s = hashmap[digits[start]];
        for (int i = 0; i < s.size(); i++)
        {
            path.push_back(s[i]);
            DFS(digits, start+1, path);
            path.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        if (digits.empty())
            return {};
        string path;
        DFS(digits, 0, path);
        return ans;
    }
};

```

<br>
