- [十五.位操作专题](#十五位操作专题)
        - [89.格雷编码](#89格雷编码)
        - [剑指 Offer 64. 求1+2+…+n](#剑指-offer-64-求12n)
        - [剑指 Offer 65. 不用加减乘除做加法](#剑指-offer-65-不用加减乘除做加法)



# 十五.位操作专题



---------------------------------
##### 89.格雷编码
>题目描述：格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。
给定一个代表编码总位数的非负整数 n，打印其格雷编码序列。即使有多个不同答案，你也只需要返回其中一种。
格雷编码序列必须以 0 开头。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/gray-code

解题思路：设置一个标志位pos（pos初始值为 1），pos在每一次遍历中都需要左移一位，然后再加到返回值的vector数组里，每一次的遍历都通过pos相加创建新的数，然后再将这些数加到返回值数组里面。

时间复杂度：O(2^n)

空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> ans{0};
        int pos = 1;
        for (int i = 1; i <= n; i++)
        {
            vector<int> temp = ans;
            for (auto it = temp.rbegin(); it != temp.rend(); it++)
            {
                *it += pos;
                ans.push_back(*it);
            }
            pos <<= 1;
        }
        return ans;
    }
};

```

<br>



---------------------------
##### 剑指 Offer 64. 求1+2+…+n
>题目描述:求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/qiu-12n-lcof

解题思路：不能使用if，那么就用 &&（逻辑与）操作的截断效应来模拟if即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int sumNums(int n) {
        int sum = 0;
        n && (sum=n+sumNums(n-1));
        return sum;
    }
};
```

<br>


---------------------------
##### 剑指 Offer 65. 不用加减乘除做加法
>题目描述:写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof

解题思路：通过位操作实现，异或操作 可以保留 相加后的余数位，与操作 左移一位后 也就是 进位。 

时间复杂度：O(1)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int add(int a, int b) {
        while (a)
        {
            int carry = ((unsigned)(a&b)<<1), sum = (a^b);
            a = carry, b = sum;
        }
        return b;
    }
};

```

<br>

