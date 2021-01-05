- [四.栈和队列专题](#四栈和队列专题)
        - [剑指 Offer 09. 用两个栈实现队列](#剑指-offer-09-用两个栈实现队列)
        - [剑指 Offer 30. 包含min函数的栈](#剑指-offer-30-包含min函数的栈)
        - [剑指 Offer 59 - II. 队列的最大值](#剑指-offer-59---ii-队列的最大值)
        - [剑指 Offer 31. 栈的压入、弹出序列](#剑指-offer-31-栈的压入弹出序列)
        - [20.有效的括号](#20有效的括号)
        - [32.最长的有效括号](#32最长的有效括号)
        - [84.柱状图中最大的矩形](#84柱状图中最大的矩形)
        - [239. 滑动窗口最大值](#239-滑动窗口最大值)
        - [150.逆波兰表达式求值](#150逆波兰表达式求值)

# 四.栈和队列专题



---------------------------
##### 剑指 Offer 09. 用两个栈实现队列
>题目描述:用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof

解题思路：设置两个队列，左边的队列用来入队，右边的队列用来出队。

时间复杂度：O(1)

空间复杂度：O(1)

```cpp
class CQueue {
    std::stack<int> stk1;
    std::stack<int> stk2;
public:
    CQueue() {
    }
    
    void appendTail(int value) {
        stk1.push(value);
    }
    
    int deleteHead() {
        if (stk2.empty())
        {
            if (stk1.empty())
                return -1;
            else
            {
                while (!stk1.empty())
                {
                    stk2.push(stk1.top());
                    stk1.pop();
                }
            }
        }
        int val = stk2.top();
        stk2.pop();
        return val;
    }
};
```

<br>


---------------------------
##### 剑指 Offer 30. 包含min函数的栈
>题目描述:定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof

解题思路：用两个栈，一个栈存放正常的元素，另一个栈保存此时元素中的最小值。

时间复杂度：min, push, pop, top 均为O(1)

空间复杂度：O(N)

```cpp
class MinStack {
    std::stack<int> stk1;
    std::stack<int> stk2;
public:
    /** initialize your data structure here. */
    MinStack() {
    }
    
    void push(int x) {
        stk1.push(x);
        if (stk2.empty())
            stk2.push(x);
        else
        {
            int val = stk2.top()<=x ? stk2.top() : x;
            stk2.push(val); 
        }
    }
    
    void pop() {
        if (!stk1.empty())
        {
            stk1.pop();
            stk2.pop();
        }
    }
    
    int top() {
        return stk1.empty() ? -1 :  stk1.top();
    }
    
    int min() {
        return stk2.empty() ? INT32_MAX : stk2.top();
    }
};
```

<br>





---------------------------
##### 剑指 Offer 59 - II. 队列的最大值
>题目描述:请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。
若队列为空，pop_front 和 max_value 需要返回 -1

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof

解题思路：用两个辅助queue，一个存放元素的值，一个作为单调递减队列

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class MaxQueue {
    std::deque<int> que1;
    std::deque<int> que2;
public:
    MaxQueue() {
    }
    
    int max_value() {
        return que2.empty() ? -1 : que2.front();
    }
    
    void push_back(int value) {
        que1.push_back(value);
        while (!que2.empty() && value>que2.back())
            que2.pop_back();
        que2.push_back(value);
    }
    
    int pop_front() {
        if (que1.empty())
            return -1;
        int val = que1.front();
        que1.pop_front();
        if (que2.front() == val)
            que2.pop_front();
        return val;
    }
};

```

<br>


---------------------------
##### 剑指 Offer 31. 栈的压入、弹出序列
>题目描述:输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof

解题思路：遍历弹栈序列，并设置辅助栈，保证每一次循环都能够进行弹栈即可，若无法弹栈，那么就将压栈序列挨个入栈。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        std::stack<int> stk;
        int idx = 0;
        for (int i = 0; i < popped.size(); i++)
        {
            while (stk.empty() || stk.top()!=popped[i])
            {
                if (idx == pushed.size())
                    return false;
                stk.push(pushed[idx++]);
            }
            stk.pop();
        }
        return true;
    }
};
```

<br>


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
##### 239. 滑动窗口最大值
>题目描述:给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。
返回滑动窗口中的最大值。
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sliding-window-maximum

解题思路：用一个单调deque作为辅助，存放的是单调递减的元素下标，当新的元素更大不满足单调减的特性时需要从队尾出队，当deque长度超长时需要从队头出队。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        if (nums.empty() || nums.size()<k )
            return {};
        std::deque<int> deque;//存储下标
        for (int i = 0; i < k; ++i)
        {
            while (!deque.empty() && nums[i]>=nums[deque.back()])
                deque.pop_back();
            deque.push_back(i);
        }
        vector<int> ans{nums[deque.front()]};
        for (int i = k; i < nums.size(); i++)
        {
            if (i - deque.front() >= k)
                deque.pop_front();
            while (!deque.empty() && nums[i]>=nums[deque.back()])
                deque.pop_back();
            deque.push_back(i);
            ans.push_back(nums[deque.front()]);
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


