- [十五.位操作专题](#十五位操作专题)
        - [136.只出现一次的数字](#136只出现一次的数字)
        - [137.只出现一次的数字2](#137只出现一次的数字2)
        - [260.只出现一次的数字3](#260只出现一次的数字3)
        - [89.格雷编码](#89格雷编码)
        - [剑指 Offer 64. 求1+2+…+n](#剑指-offer-64-求12n)
        - [剑指 Offer 65. 不用加减乘除做加法](#剑指-offer-65-不用加减乘除做加法)



# 十五.位操作专题


-------------------------------------------
##### 136.只出现一次的数字
>题目描述：给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/single-number

解题思路：对整个数组中的元素进行异或操作，最后剩下的值就是只出现一次的元素。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (auto& i : nums)
            ans ^= i;
        return ans;
    }
};
```

<br>

-------------------------------------------
##### 137.只出现一次的数字2
>题目描述：给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/single-number-ii

解题思路：位运算，每个数都有32位，从最低位0位开始，遍历数组累积该位上的数在%3，也就求得了该位上有用的数，按照此方法遍历32位再累加即可求得只出现了一次的元素。该方法同样可以求解类似问题：有一个元素出现了一次，其余元素出现了k（k为奇数且不为1）次。

时间复杂度：O(32*N) ≈ O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int i = 0; i < 32; i++)
        {
            int cnt = 0;
            for (auto& e: nums)
                if (e & (1<<i))
                    cnt++;
            cnt %= 3;
            if (cnt)
                ans += (1<<i);
        }
        return ans;
    }
};

```

<br>

----------------------------
##### 260.只出现一次的数字3
>题目描述：给定一个整数数组 nums，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。
结果输出的顺序并不重要。
你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/single-number-iii

解题思路：先对整个数组进行异或操作，最后剩下的元素是求解的两个元素异或的结果，找到该结果中为1的某一位，通过该位将整个数组分为两个部分，每个部分各自进行异或操作也就可以得到求解的两个元素了。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int target = 0;
        for (auto& e: nums)
            target ^= e;
        int i = 1;
        while (!(i&target))
            i <<= 1;
        int res1 = 0, res2 = 0;
        for (auto& e: nums)
        {
            if (i & e)
                res1 ^= e;
            else
                res2 ^= e;
        }
        return vector<int>{res1, res2};
    }
};

```

<br>


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

