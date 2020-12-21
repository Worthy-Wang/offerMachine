- [十.分治法专题](#十分治法专题)
        - [50.Pow(x,n)](#50powxn)
        - [69.x的平方根](#69x的平方根)


# 十.分治法专题

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
    double Pow(double x, long long n)
    {
        double res = 1;
        while (n)
        {
            if (n & 0x1)
                res *= x;
            x *= x;
            n >>= 1;
        }
        return res;
    }

    double myPow(double x, int n) {
        if (0 == n)
            return 1;
        if (1 == x)
            return 1;
        long long N = n;
        if (n < 0)
            return 1.0 / Pow(x, -N);
        else
            return Pow(x, N);
    }
};

```

<br>


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
        long long l = 1, r = x, X = x;
        while (l <= r)
        {
            long long mid = (l + r) >> 1;
            if (mid*mid == X)
                return mid;
            else if (mid*mid < X)
                l = mid+1;
            else
                r = mid-1;
        }   
        return r;
    }
};

```

<br>

