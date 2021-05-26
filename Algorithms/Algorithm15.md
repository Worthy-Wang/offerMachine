- [十五.细节实现专题](#十五细节实现专题)
    - [66.加一](#66加一)
    - [7.整数反转](#7整数反转)
    - [343. 整数拆分](#343-整数拆分)
    - [9.回文数](#9回文数)
    - [56.合并区间](#56合并区间)
    - [57.插入区间](#57插入区间)
    - [68.文本左右对齐](#68文本左右对齐)
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
        int carry = 1;
        int i = digits.size() - 1;
        while (0 <= i)
        {
            int sum =  digits[i] + carry;
            digits[i] = sum % 10;
            carry = sum / 10;
            --i;
            if (!carry)
                break;
        }
        if (carry)
            digits.insert(digits.begin(), 1);
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
##### 343. 整数拆分
>题目描述：给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/integer-break

解题思路：动态规划, dp[i]代表拆分整数i能够获得的最大乘积

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int integerBreak(int n) {
        if (n <= 1)
            return 0;
        vector<int> dp(n + 1, 0);
        dp[1] = 1;
        for (int i = 2; i <= n; ++i)
            for (int j = 1; j < i; ++j)
            {
                int l = std::max(j , dp[j]);
                int r = std::max(i-j, dp[i-j]);
                dp[i] = std::max(dp[i], l * r);
            }
        return dp[n];
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
