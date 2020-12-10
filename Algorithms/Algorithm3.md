- [三.栈和队列专题](#三栈和队列专题)
        - [20.有效的括号](#20有效的括号)
        - [32.最长的有效括号](#32最长的有效括号)
        - [84.柱状图中最大的矩形](#84柱状图中最大的矩形)
        - [150.逆波兰表达式求值](#150逆波兰表达式求值)

# 三.栈和队列专题

---------------------------
##### 20.有效的括号
>题目描述:给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
有效字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-parentheses

解题思路：建立辅助栈，for循环遍历字符串，当 栈为空 或者 元素与栈顶不匹配 的时候继续入栈，反之出栈；最后根据栈是否为空判断该字符串的有效性。另外判断括号匹配需要建立哈希表。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char,char> hashmap{{'(',')'}, {'{','}'}, {'[', ']'}};
        std::stack<char> stk;
        for (auto& e: s)
        {
            if (stk.empty() ||  e!=hashmap[stk.top()])
                stk.push(e);
            else
                stk.pop();
        }
        return stk.empty();
    }
};

```

<br>


---------------------------
##### 32.最长的有效括号
>题目描述:给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。
示例 1:
输入: "((()))"
输出: 3
解释: 最长有效括号子串为 "((()))"
示例 2:
输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"
示例 3:
输入: "()(())"
输出: 6
解释: 最长有效括号子串为 "()(())"

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-valid-parentheses

解题思路：先通过模拟栈的方法，记录不能够进行匹配消除的所有 '(' 的下标置位为1，然后该问题就进行了转换，也就是在只有0 和 1 的数组中求 连续0 的最大长度，可以以 1 作为界限，计算中间区域的长度。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        std::stack<int> stk;
        for (int i = 0; i < s.size(); i++)
        {
            if (stk.empty())
                stk.push(i);
            else if ('('==s[stk.top()] && ')'==s[i])
                stk.pop();
            else
                stk.push(i);
        }

        //计算stk中的最大间隔
        int maxLen = 0;
        int r = s.size(), l;
        while(!stk.empty())
        {
            l = stk.top();
            stk.pop();
            maxLen = std::max(maxLen, r-l-1);
            r = l;
        }
        l = -1;
        maxLen = std::max(maxLen, r-l-1);
        return maxLen;
    }
};


```

<br>


---------------------------
##### 84.柱状图中最大的矩形
>题目描述:给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
求在该柱状图中，能够勾勒出来的矩形的最大面积。
示例:
输入: [2,1,5,6,2,3]
输出: 10

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/largest-rectangle-in-histogram

* **解法一**

解题思路：暴力法，两层for循环，采用中心扩散法，第一次遍历中的柱子作为最低的高度向两边扩散。

时间复杂度：O(N^2)

空间复杂度：O(1)


* **解法二**

解题思路：单调栈法，在暴力法的基础上进行改进。在暴力法中是以当前柱体为最低高度向左右两边展开，需要分别找到 左边第一个小于当前柱体高度的 和 右边第一个小于当前柱体高度的。而单调栈可以帮助我们完成该任务，当 当前柱体的高 小于 栈顶的高时，栈顶出栈，这样就同时找到了左右两边的更小值。注意此处操作的节点是 弹出栈的那一个。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        if (heights.empty())
            return 0;
        int ans = 0;
        heights.push_back(0);//在数组尾部加入一个最小的数，保证单调栈中的所有数都可以进行计算
        std::stack<int> stk;//单调栈存储的是下标
        for (int i = 0; i < heights.size(); i++)
        {
            while (!stk.empty() && heights[i]<heights[stk.top()])
            {
                int mid = stk.top();
                stk.pop();
                int l = stk.empty() ? -1 : stk.top();
                int r = i;
                ans = std::max(ans, heights[mid]*(r-l-1));
            }
            stk.push(i);
        }
        return ans;
    }
};

```

<br>

---------------------------
##### 150.逆波兰表达式求值
>题目描述:
根据 逆波兰表示法，求表达式的值。
有效的运算符包括 +, -, *, / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。
说明：
整数除法只保留整数部分。
给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。
逆波兰表达式：
逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。
平常使用的算式则是一种中缀表达式，如 ( 1 + 2 ) * ( 3 + 4 ) 。
该算式的逆波兰表达式写法为 ( ( 1 2 + ) ( 3 4 + ) * ) 。
逆波兰表达式主要有以下两个优点：
去掉括号后表达式无歧义，上式即便写成 1 2 + 3 4 + * 也可以依据次序计算出正确结果。
适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/evaluate-reverse-polish-notation

解题思路：逆波兰表达式的核心在于遇到数字时进行入栈，遇到运算符号时出栈两个元素，并将其计算的结果入栈，最后弹出栈中剩下的最后一个元素即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        if (tokens.empty())
            return 0;
        std::stack<int> stk;
        for(auto& e: tokens)
        {
            if (e.size()==1 && !isdigit(e[0]))
            {
                int b = stk.top();
                stk.pop();
                int a = stk.top();
                stk.pop();
                switch(e[0])
                {
                    case '+': stk.push(a+b); break;
                    case '-': stk.push(a-b); break;
                    case '*': stk.push(a*b); break;
                    case '/': stk.push(a/b); break;
                    default: break;
                }
            }
            else 
                stk.push(stoi(e));
        }
        int sum = stk.top();
        return sum;
    }
};

```

<br>


