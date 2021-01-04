- [二.字符串专题](#二字符串专题)
        - [剑指 Offer 05. 替换空格](#剑指-offer-05-替换空格)
        - [125.验证回文串](#125验证回文串)
        - [28.实现strStr()](#28实现strstr)
        - [剑指 Offer 20. 表示数值的字符串](#剑指-offer-20-表示数值的字符串)
        - [8.字符串转换整数](#8字符串转换整数)
        - [67.二进制求和](#67二进制求和)
        - [10.正则表达式匹配](#10正则表达式匹配)
        - [44.通配符匹配](#44通配符匹配)
        - [65.有效数字](#65有效数字)
        - [12.整形转罗马数字](#12整形转罗马数字)
        - [13.罗马数字转整形](#13罗马数字转整形)
        - [38.外观数列](#38外观数列)
        - [49.字母异位词分组](#49字母异位词分组)
        - [71.简化路径](#71简化路径)
        - [58.最后一个单词的长度](#58最后一个单词的长度)


# 二.字符串专题


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

<br>


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
void getNext(string &needle, vector<int> &next)
{
    int n = needle.size();
    next[0] = -1;
    int i = 0, j = -1; //i在后，j在前
    while (i < n)
    {
        if (-1 == j || needle[i] == needle[j])
        {
            i++, j++;
            next[i] = j;
        }
        else
            j = next[j];
    }
}

int strStr(string haystack, string needle)
{
    int n1 = haystack.size(), n2 = needle.size();
    if (0==n2)
        return 0;
    if (0==n1) 
        return -1;
    vector<int> next(n2+1);//+1是因为构建next数组的过程中会多构建一个无用的下标
    getNext(needle, next);
    int i = 0, j = 0; //i作为主串的下标，j作为子串的下标
    while (i < n1 && j<n2)
    {
        if (-1 == j || haystack[i] == needle[j])
        {
            i++, j++;
            if (j == n2)
                return i - n2;
        }
        else
            j = next[j];
    }
    return -1;
}
};

```

<br>


---------------------------
##### 剑指 Offer 20. 表示数值的字符串
>题目描述:请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"-1E-16"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。

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
        int n1 = a.size(), n2 = b.size();
        if (n1 < n2)
        {
            for (int i = 0; i < n2-n1; i++)
                a = '0' + a;
        }
        else
        {
            for (int i = 0; i < n1-n2; i++)
                b = '0' + b;
        }

        string ans;
        int carry = 0;
        for (int i = std::max(n1,n2)-1; i >= 0; i--)
        {
            int sum = a[i]-'0' + b[i]-'0' + carry;
            ans = to_string(sum%2) + ans;
            carry = (sum>=2 ? 1 : 0);
        }
        if (carry)
            ans = '1' + ans;
        return ans;
    }
};
```

<br>


-----------------------------
##### 10.正则表达式匹配
>题目描述：

解题思路：

时间复杂度：

空间复杂度：

```cpp

```

<br>


-----------------------------
##### 44.通配符匹配
>题目描述：

解题思路：

时间复杂度：

空间复杂度：

```cpp

```

<br>





-----------------------------
##### 65.有效数字
>题目描述：验证给定的字符串是否可以解释为十进制数字。
说明: 我们有意将问题陈述地比较模糊。在实现代码之前，你应当事先思考所有可能的情况。这里给出一份可能存在于有效十进制数字中的字符列表。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-number

解题思路：首先考虑一种最复杂的情况 "  -3.1415e13 "  ，我们将 . 作为一个区分处，只要 . 的前后有合法的数字那么该数字即是合法的；另外如果后面有 e(E) 的话，后面必须接上数字才行，最后再判断是否有其他的非法字母。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isNumber(string s) {
        int i = 0;
        while (isspace(s[i])) i++; //忽略空格
        if ('+'==s[i] || '-'==s[i])
            i++;
        bool flag1=  false;
        while (isdigit(s[i]))
        {
            flag1 = true;
            i++;    
        }
        if ('.' == s[i])
            i++;
        bool flag2 = false;
        while (isdigit(s[i]))
        {
            flag2 = true;
            i++;
        }
        if (!flag1 && !flag2)
            return false;
        
        // 如果有e(E)的话
        if ('e'==s[i] || 'E'==s[i])
        {
            i++;
            if ('+'==s[i] || '-'==s[i])
                i++;
            bool flag3 = false;
            while (isdigit(s[i]))
            {
                flag3 = true;
                i++;
            }
            if (!flag3)
                return false;
        }

        for (; i < s.size(); i++)
            if (!isspace(s[i]))
                return false;
        return true;        
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
        unordered_map<string, int> hashmap{{"I",1}, {"IV", 4}, {"V", 5}, {"IX", 9}, {"X", 10}, {"XL", 40}, {"L", 50}, {"XC", 90}, {"C",100}, {"CD",400}, {"D", 500}, {"CM",900}, {"M", 1000}};
        int sum = 0;
        for (int i = 0; i < s.size(); i++)
        {
            if (i==s.size()-1)
            {
                sum += hashmap[s.substr(i,1)];
            }
            else if (hashmap.count(s.substr(i, 2)))
            {
                sum += hashmap[s.substr(i,2)];
                i++;
            }else   
            {
                sum += hashmap[s.substr(i,1)];
            }
        }
        return sum;
    }
};
```

<br>


-----------------------------
##### 38.外观数列
>题目描述：给定一个正整数 n ，输出外观数列的第 n 项。
「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。
你可以将其视作是由递归公式定义的数字字符串序列：
countAndSay(1) = "1"
countAndSay(n) 是对 countAndSay(n-1) 的描述，然后转换成另一个数字字符串。
前五项如下：
1.     1
2.     11
3.     21
4.     1211
5.     111221
第一项是数字 1 
描述前一项，这个数是 1 即 “ 一 个 1 ”，记作 "11"
描述前一项，这个数是 11 即 “ 二 个 1 ” ，记作 "21"
描述前一项，这个数是 21 即 “ 一 个 2 + 一 个 1 ” ，记作 "1211"
描述前一项，这个数是 1211 即 “ 一 个 1 + 一 个 2 + 二 个 1 ” ，记作 "111221"

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/count-and-say

解题思路：动态规划，dp[n] 总是通过 dp[n-1]计算得出，在此题中用两个string代替即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string countAndSay(int n)
    {
        string ans("1");
        for (int k = 0; k < n - 1; k++)
        {
            stringstream ss(ans);
            auto l = ans.begin();
            while (l != ans.end())
            {
                auto r = std::find_if(l, ans.end(), [&](char ch) {
                    return ch != (*l);
                });
                int cnt = distance(l, r);
                ss <<  cnt << *l;
                l = r;
            }
            ans = ss.str();
        }
        return ans;
    }
};

```

<br>


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

<br>

