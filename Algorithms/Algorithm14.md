## 目录
- [十四.位操作专题](#十四位操作专题)
    - [50.Pow(x,n)](#50powxn)
    - [69.x的平方根](#69x的平方根)
    - [136.只出现一次的数字](#136只出现一次的数字)
    - [137.只出现一次的数字2](#137只出现一次的数字2)
    - [260.只出现一次的数字3](#260只出现一次的数字3)
    - [89.格雷编码](#89格雷编码)
    - [剑指 Offer 64. 求1+2+…+n](#剑指-offer-64-求12n)
    - [剑指 Offer 65. 不用加减乘除做加法](#剑指-offer-65-不用加减乘除做加法)
    - [29.两数相除](#29两数相除)
    - [191. 位1的个数](#191-位1的个数)



### 十四.位操作专题

---------------------------
##### 50.Pow(x,n)
>题目描述:实现 pow(x, n) ，即计算 x 的 n 次幂函数。
-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−2^31, 2^31 − 1] 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/powx-n

解题思路：基于二进制的快速幂运算，x^n 求解，将 n 转化为二进制数，不断在二进制数的n末尾判断是1还是0，是1的话说明可以进行累乘，然后不断右移1位即可。注意x也是不断进行乘积的。

时间复杂度：O(logN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    double Pow(double x, long n)
    {   
        double ans = 1;
        for (int i = 0; i < 32; ++i){
            if ((1 << i) & n)
            {
                ans *= x;
            }            
            x *= x;
        }
        return ans;
    }

    double myPow(double x, int n) {
        if (0 == n)
            return 1;
        if (1 == x)
            return 1;
        long N = n;

        if (N > 0)
            return Pow(x, N);
        else
            return 1.0 / Pow(x, -N);
    }
};

```

<br>





<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------
##### 69.x的平方根
>题目描述:实现 int sqrt(int x) 函数。
计算并返回 x 的平方根，其中 x 是非负整数。
由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sqrtx

解题思路：已经暴力法，从 x/2 开始寻找 k ，找到第一个 K*k<=x 的数，该数即为x的平方根；在此基础上使用二分查找即可。

时间复杂度：O(logX)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int l = 0, r = x;
        while (l <= r)
        {
            long mid = (l + r) >> 1;
            if (mid*mid == x)
                return mid;
            else if (mid*mid < x)
                l = mid + 1;
            else 
                r = mid - 1;
        }
        return l - 1;
    }
};

```

<br>




<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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

<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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

<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


---------------------------------
##### 89.格雷编码
>题目描述：格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。
给定一个代表编码总位数的非负整数 n，打印其格雷编码序列。即使有多个不同答案，你也只需要返回其中一种。
格雷编码序列必须以 0 开头。
输入: 2
输出: [0,1,3,2]
解释:
00 - 0
01 - 1
11 - 3
10 - 2
对于给定的 n，其格雷编码序列并不唯一。
例如，[0,2,3,1] 也是一个有效的格雷编码序列。
00 - 0
10 - 2
11 - 3
01 - 1

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



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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


<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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
        long a = abs(dividend), b = abs(divisor);
        long cnt = 0;
        for (int i = 31; i >= 0; --i)
        {
            if (a >= b * (unsigned)(1 << i))
            {
                a -= b * (unsigned)(1 << i);
                cnt += (unsigned)(1 << i);
            }
        }

        if ((dividend>0 && divisor >0) || (dividend<0 && divisor<0))
            cnt = cnt;
        else
            cnt = -cnt;
        if (cnt > INT32_MAX || cnt < INT32_MIN)
            return INT32_MAX;
        return cnt;
    }
};

```

<br>



<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


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


当然，按位与的方法也可以解决：
```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int cnt = 0;
        for (int i = 0; i < 32; ++i)
            if ((1 << i) & n)
                cnt ++;
        return cnt;
    }
};

```


<br>

<div align="right">
    <b><a href="#目录">↥ Back To Top</a></b>
</div>


