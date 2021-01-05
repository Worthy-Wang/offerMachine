- [二.链表专题](#二链表专题)
        - [2.两数相加](#2两数相加)
        - [445.两数相加2](#445两数相加2)
        - [206.反转链表](#206反转链表)
        - [92.反转链表2](#92反转链表2)
        - [86.分隔链表](#86分隔链表)
        - [83.删除排序链表中的重复元素](#83删除排序链表中的重复元素)
        - [82.删除排序链表中的重复元素2](#82删除排序链表中的重复元素2)
        - [61.旋转链表](#61旋转链表)
        - [19.删除链表的倒数第N个节点](#19删除链表的倒数第n个节点)
        - [24.两两交换链表中的节点](#24两两交换链表中的节点)
        - [25.K个一组翻转链表](#25k个一组翻转链表)
        - [138.复制带随机指针的链表](#138复制带随机指针的链表)
        - [141.环形链表](#141环形链表)
        - [142.环形链表2](#142环形链表2)
        - [876.链表的中间节点](#876链表的中间节点)
        - [剑指 Offer 52. 两个链表的第一个公共节点](#剑指-offer-52-两个链表的第一个公共节点)
        - [143.重排链表](#143重排链表)
        - [146.LRU缓存机制](#146lru缓存机制)


# 二.链表专题


----------------------------
##### 2.两数相加
>题目描述：给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers

解题思路：设置双指针同时遍历，并设置carry进位，返回一个新的链表。

时间复杂度：O(max(M+N))

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* tail = dummy;
        int carry = 0;
        while (l1 || l2 || carry)
        {
            int val1 = (l1 ? l1->val : 0);
            int val2 = (l2 ? l2->val : 0);
            int sum = val1 + val2 + carry;
            carry = (sum >= 10 ? 1 : 0); 
            ListNode* node = new ListNode(sum%10);
            tail->next = node;
            tail = node;
            l1 = (l1 ? l1->next : nullptr);
            l2 = (l2 ? l2->next : nullptr);
        }
        return dummy->next;
    }
};

```

<br>

--------------------------
##### 445.两数相加2
>题目描述：给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
你可以假设除了数字 0 之外，这两个数字都不会以零开头。
如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers-ii

* **解法一**

解题思路：先分别遍历两个链表，用两个数分别存放两个链表的值，将两个数相加，再创建新的链表存放结果即可。此方法运行无法通过，因为链表过长的情况下是无法用某一个数字进行存储的 ！

时间复杂度：O(M+N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int sum1 = 0, sum2 = 0;
        for (auto p = l1; p; p = p->next)
            sum1 = sum1 * 10 + p->val;
        for (auto p = l2; p; p = p->next)
            sum2 = sum2 * 10 + p->val;
        string s(to_string(sum1 + sum2));
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        for (auto& i: s)
        {
            ListNode* node = new ListNode(i-'0');
            tail->next = node;
            tail = node;
        }
        return dummy.next;
    }
};
```

* **解法二**

解题思路：逆序想到双栈法，分别用两个栈存放两个链表的值，再通过弹栈创建新的链表，注意由于是逆序创建，需要使用头插法创建链表。

时间复杂度：O(M+N)

空间复杂度：O(M+N)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> stk1, stk2;
        for (auto p = l1; p; p=p->next)
            stk1.push(p->val);
        for (auto p = l2; p; p=p->next)
            stk2.push(p->val);
        ListNode dummy(-1);
        ListNode *tail = &dummy;
        int carry = 0;
        while (!stk1.empty() || !stk2.empty() || carry)
        {
            int val1 = (stk1.empty() ? 0 : stk1.top());
            int val2 = (stk2.empty() ? 0 : stk2.top());
            if (!stk1.empty()) stk1.pop();
            if (!stk2.empty()) stk2.pop();
            int sum = carry + val1 + val2;
            carry = (sum>=10 ? 1 : 0);
            //头插法
            ListNode *node = new ListNode(sum%10);
            node->next = dummy.next;
            dummy.next = node;
        }
        return dummy.next;
    }
};
```


<br>

-----------------------------------
##### 206.反转链表
>题目描述：反转一个单链表。
示例:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list

* **解法一**

解题思路：迭代法，一边遍历链表一边使用头插法插入。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode dummy(-1);
        ListNode *cur = head;
        while (cur)
        {
            ListNode *temp = cur->next;
            cur->next = dummy.next;
            dummy.next = cur;
            cur = temp;
        }   
        return dummy.next;
    }
};

```

* **解法二**

解题思路：递归法

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode* ret = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return ret;
    }
};
```


<br>

--------------------------
##### 92.反转链表2
>题目描述：反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。
说明:
1 ≤ m ≤ n ≤ 链表长度。
示例:
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list-ii

解题思路：先遍历到位置m的前一个节点，让链表从此处断开称为两个链表，然后不断将第二个链表中的节点通过头插法加入到第一个链表中。

时间复杂度：O(链表长度)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (!head || !head->next || m==n)
            return head;
        ListNode dummy(-1);
        dummy.next = head;
        ListNode *pre = &dummy;//pre指向第m节点的前一个
        for (int i = 0; i < m-1; i++)
            pre = pre->next;
        ListNode *cur = pre->next, *rHead = pre->next;
        pre->next = nullptr;//将两个链表断开，更方便理解
        for (int i = m; i <= n; i++)
        {
            ListNode* temp = cur->next;
            cur->next = pre->next;
            pre->next = cur;
            cur = temp;
        }
        rHead->next = cur;
        return dummy.next;
    }
};

```

<br>

-----------------------------------
##### 86.分隔链表
>题目描述：给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。
你应当保留两个分区中每个节点的初始相对位置。
示例:
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/partition-list

解题思路：设置左右两个头结点，遍历链表，若元素值小于x则添加到左边链表，大于x则添加到右边链表；最后将两个链表连接起来。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode leftDummy(-1), rightDummy(-1);
        ListNode* leftTail = &leftDummy, *rightTail = &rightDummy;
        for (auto p = head; p; p=p->next)
        {
            if (p->val < x)
            {
                leftTail->next = p;
                leftTail = p;
            }
            else
            {
                rightTail->next = p;
                rightTail = p;
            }
        }
        leftTail->next = rightDummy.next;
        rightTail->next = nullptr;
        return leftDummy.next;
    }
};

```

<br>

--------------------------------------
##### 83.删除排序链表中的重复元素
>题目描述：给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

解题思路：设置头结点，一次遍历链表即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode dummy(-1);
        ListNode *tail = &dummy;
        for (auto p = head; p; p=p->next)
        {
            if (tail==&dummy || tail->val!=p->val)
            {
                tail->next = p;
                tail = p;
            }
            else
                continue;
        }
        tail->next = nullptr;
        return dummy.next;
    }
};

```

<br>

-----------------------------------
##### 82.删除排序链表中的重复元素2
>题目描述：给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii

解题思路：使用一次遍历即可求解，先需要设置一个tail指针通过尾插法用来连接后续没有重复的节点，再在tail指针后面使用 快慢双指针 判断节点是否重复。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        ListNode *s = head;
        while (s)
        {
            ListNode* f = s->next;
            if (f && f->val==s->val)
            {
                while(f && f->val==s->val)
                    f = f->next;
                s = f;
            }   
            else
            {
                tail->next = s;
                tail = s;
                s = s->next;
            } 
        }
        tail->next = nullptr;
        return dummy.next;
    }
};

```

<br>

-----------------------------------
##### 61.旋转链表
>题目描述：给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。
示例 1:
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-list

解题思路：先遍历整个链表将链表的长度len测量出来，并将链表的首尾相连；len-k%len 处断开，即可求得新的链表

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next)
            return head;
        int len = 1;
        auto cur = head;
        for (; cur->next; cur=cur->next)
            len++;
        cur->next = head;//成环
        
        int pos = len - k % len;
        cur = head;
        for (int i = 0; i < pos-1; i++)
            cur = cur->next;
        ListNode* start = cur->next;
        cur->next = nullptr;
        return start;
    }
};

```

<br>

-----------------------------------
##### 19.删除链表的倒数第N个节点
>题目描述：给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
示例：
给定一个链表: 1->2->3->4->5, 和 n = 2.
当删除了倒数第二个节点后，链表变为 1->2->3->5.
给定的 n 保证是有效的。
你能尝试使用一趟扫描实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list

解题思路：快慢指针法，快指针先走n步，接着快慢指针同时走，快指针走到末尾时慢指针即可删除倒数第n个节点。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if (!head  || n<=0)
            return head;
        ListNode dummy(-1);
        dummy.next = head;
        ListNode *s = &dummy, *f = &dummy;
        for (int i = 0; i < n; i++)
            f = f->next;
        for (; f->next ; f=f->next)
            s = s->next;
        s->next = s->next->next;
        return dummy.next;
    }
};
```

<br>

-----------------------------------
##### 24.两两交换链表中的节点
>题目描述：
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
示例 1：
输入：head = [1,2,3,4]
输出：[2,1,4,3]
示例 2：
输入：head = []
输出：[]
示例 3：
输入：head = [1]
输出：[1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/swap-nodes-in-pairs


解题思路：只进行一次遍历即可，设置pre指针，l指针，r指针；pre指针在前，l，r指针指向需要交换的两个节点，不断往后迭代。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode dummy(-1);
        dummy.next = head;
        ListNode* pre = &dummy, *l = pre->next, *r = l->next;
        while  (1)
        {
            l->next = r->next;
            pre->next = r;
            r->next = l;
            
            pre = l;
            l = pre->next;
            if (!l) break;
            r = l->next;
            if (!r) break;
        }
        return dummy.next;
    }
};

```

<br>

-----------------------------------
##### 25.K个一组翻转链表
>题目描述：给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。
示例：
给你这个链表：1->2->3->4->5
当 k = 2 时，应当返回: 2->1->4->3->5
当 k = 3 时，应当返回: 3->2->1->4->5
你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-nodes-in-k-group

解题思路：对链表进行k个k个的遍历，先专门写一个函数用于转置链表的k个元素（需要返回新的首尾节点），该题相当于是 翻转链表 题的难度加大题。该题的关键点在于设置好指针，指向 上一个链表的尾结点 和 下一个链表的头结点。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    pair<ListNode*, ListNode*> reverseList(ListNode* head, ListNode* tail)
    {
        ListNode dummy(-1);
        dummy.next = head;
        ListNode* cur = head, *end = tail->next;
        while (cur != end)
        {
            auto t = cur->next;
            cur->next = dummy.next;
            dummy.next = cur;
            cur = t;
        }
        return make_pair(tail, head);
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        if (k<=1 || !head || !head->next)
            return head;
        ListNode dummy(-1);
        dummy.next = head;
        ListNode* pre = &dummy;
        while (1)
        {
            ListNode* cur = pre;
            for (int i = 0; i < k; i++)
            {
                cur = cur->next;
                if (!cur)
                    return dummy.next;
            }
            ListNode* rightHead = cur->next;
            pair<ListNode*,ListNode*> newPair = reverseList(pre->next, cur);
            ListNode* newHead = newPair.first, *newTail = newPair.second;
            pre->next = newHead;
            pre = newTail;
            newTail->next = rightHead;
        }
        return dummy.next;
    }
};

```

<br>

-----------------------------------
##### 138.复制带随机指针的链表
>题目描述：给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。
要求返回这个链表的 深拷贝。 
我们用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：
val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/copy-list-with-random-pointer

* **解法一**

解题思路：哈希表作为辅助，key为旧节点的指针，value为新创建的节点的指针，一次遍历先创建新节点并记录哈希表，再一次遍历将随机指针填充上即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        unordered_map<Node*, Node*> hashmap;
        Node dummy(-1);
        Node* tail = &dummy;
        for (auto cur = head; cur ; cur=cur->next)
        {
            Node* node = new Node(cur->val);
            tail->next = node;
            tail = node;
            hashmap[cur] = node;
        }
        for (auto p = head, q=dummy.next; p&&q; p=p->next, q=q->next)
            q->random = hashmap[p->random];
        return dummy.next;
    }
};

```

* **解法二**

解题思路：将深拷贝的节点存放在旧节点的next后，新节点的next又连接原来旧节点的下一个节点，这样可以起到模拟哈希表的作用。第一次遍历先创建新节点，第二次遍历填充新节点的 random指针，第三次遍历将新旧链表分开。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head)
            return nullptr;
        //创建新链表
        auto cur = head;
        while(cur)
        {
            Node* node = new Node(cur->val);
            node->next = cur->next;
            cur->next = node;
            cur = cur->next->next;
        }
        //填充新链表的random指针
        auto p = head, q = head->next;
        while (p && q)
        {
            q->random = (p->random? p->random->next : nullptr);
            p = p->next->next;
            if (q->next)
                q = q->next->next;
        }
        //将 新旧链表拆开
        Node* head2 = head->next;
        p = head, q =head2;
        while (p && q)
        {
            p->next = q->next;
            p = p->next;
            q->next = (p ? p->next : nullptr);
            q = q->next;
        }
        return head2;
    }
};

```

<br>

-----------------------------------
##### 141.环形链表
>题目描述：给定一个链表，判断链表中是否有环。
如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。
如果链表中存在环，则返回 true 。 否则，返回 false 。
你能用 O(1)（即，常量）内存解决此问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle

解题思路：快慢指针法，快指针走的速度是慢指针的两倍，如果两个指针能够重新相遇，那么说明有环；若快指针走到末尾，则说明无环。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (!head)
            return false;
        ListNode* s = head, *f = head;
        do 
        {
            if (!f || !f->next)
                return false;
            s = s->next;
            f = f->next->next;
        }while (s != f);
        return true;
    }
};

```

<br>

-----------------------------------
##### 142.环形链表2
>题目描述：给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。
说明：不允许修改给定的链表。
你是否可以使用 O(1) 空间解决此题？
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle-ii

解题思路：使用快慢指针法，快指针的速度是慢指针的两倍；让两指针相遇时，再将快指针放到初始位置处再让两个指针以相同的速度走，再次相遇的地方即为环的入口。
假设a为环之前的那段路，b为环的长度
快指针走的路程：a+xb，慢指针走的路程:a+yb，相减可知相遇时慢指针相当于走了n圈b的长度，也就是n*b，再+a 也就是等于 a + n*b = a。即可求解

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (!head)
            return head;
        ListNode* s = head, *f = head;
        do 
        {
            if (!f || !f->next)
                return nullptr;
            s = s->next;
            f = f->next->next;
        }while (s != f);
        f = head;
        while (s != f)
        {
            s = s->next;
            f = f->next;
        }
        return s;
    }
};

```

<br>

-----------------------------------
##### 876.链表的中间节点
>题目描述：给定一个头结点为 head 的非空单链表，返回链表的中间结点。
如果有两个中间结点，则返回第二个中间结点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/middle-of-the-linked-list

解题思路：快慢指针法

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode* s = head, *f = head;
        while (1)
        {
            if (!f || !f->next)
                return s;
            s = s->next;
            f = f->next->next;
        }
        return s;
    }
};

```

<br>



---------------------------
##### 剑指 Offer 52. 两个链表的第一个公共节点
>题目描述:输入两个链表，找出它们的第一个公共节点。
如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof

解题思路：链表l1走完后去走l2，链表l2走完后去走l1，两个链表的指针同时开始走，由于 两个链表的长度相加始终相同，所以两个链表一定会同时走到最后，那么也就可以找到第一个公共节点。

时间复杂度：O(M+N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (!headA || !headB)
            return nullptr;
        ListNode* l1 = headA, *l2 = headB;
        while (l1 != l2)
        {
            l1 = l1 ? l1->next : headB;
            l2 = l2 ? l2->next : headA;
        }
        return l1;
    }
};
```

<br>

-----------------------------------
##### 143.重排链表
>题目描述：给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
示例 1:
给定链表 1->2->3->4, 重新排列为 1->4->2->3.
示例 2:
给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reorder-list

解题思路：先通过找到链表的中点将其分成两个链表，再将右边的链表进行反转，再将翻转之后的右边链表依次加入到左边链表元素的右边一个。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    //寻找中间节点，若有两个中间节点则返回第一个
    ListNode* findMid(ListNode*  head)
    {
        auto s = head, f = head;
        while (f)
        {
            if (!f || !f->next || !f->next->next)
                return s;
            s = s->next;
            f = f->next->next;
        }
        return s;
    }

    //翻转链表，并返回新的首节点
    ListNode* reverseList(ListNode* head)
    {
        ListNode dummy(-1);
        auto cur = head;
        while (cur)
        {
            auto temp = cur->next;
            cur->next = dummy.next;
            dummy.next = cur;
            cur = temp;
        }
        return dummy.next;
    }

    void reorderList(ListNode* head) {
        if (!head || !head->next)
            return;
        ListNode* mid = findMid(head);
        ListNode* rHead = mid->next;
        mid->next = nullptr;
        rHead = reverseList(rHead);
        auto p = head, q = rHead;
        while (p && q)
        {
            ListNode* temp = q->next;
            q->next = p->next;
            p->next = q;
            p = p->next->next;
            q = temp;
        }
    }
};

```

<br>

-----------------------------------
##### 146.LRU缓存机制
>题目描述：运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制 。
实现 LRUCache 类：
LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
你是否可以在 O(1) 时间复杂度内完成这两种操作？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lru-cache

解题思路：list + 哈希表辅助法，用list存储元素值，越靠前代表越先进来的，越靠后代表越后进来的；再用哈希表存储key值，映射list对应的迭代器，这样可以快速的进行删除和转移。这样能够保证get 和 push 都是 O(1)的时间复杂度。

时间复杂度：O(1)

空间复杂度：O(N)


```cpp
class LRUCache {
    int _capacity;
    std::list<pair<int,int>> list;
    std::unordered_map<int, std::list<pair<int,int>>::iterator> hashmap; 
public:
    LRUCache(int capacity) {
        _capacity = capacity;
    }
    
    int get(int key) {
        if (hashmap.count(key))
        {
            auto it = hashmap[key];
            list.splice(list.end(), list, it);
            hashmap[key] = prev(list.end());
            return list.rbegin()->second; 
        }
        else
            return -1;
    }
    
    void put(int key, int value) {
        if (hashmap.count(key))
        {
            auto it = hashmap[key];
            list.splice(list.end(), list, it);
            list.rbegin()->second = value;
            hashmap[key] = prev(list.end());
        }
        else
        {
            if (_capacity == list.size())
            {
                hashmap.erase(list.begin()->first);
                list.pop_front();
            }
            list.push_back(make_pair(key, value));
            hashmap[key] = prev(list.end());
        }
    }
};

```


