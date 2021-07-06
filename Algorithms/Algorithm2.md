- [二.链表专题](#二链表专题)
    - [21.合并两个有序链表](#21合并两个有序链表)
    - [23.合并K个升序链表](#23合并k个升序链表)
    - [147.对链表进行插入排序](#147对链表进行插入排序)
    - [148.排序链表](#148排序链表)
    - [2.两数相加](#2两数相加)
    - [445.两数相加2](#445两数相加2)
    - [206.反转链表](#206反转链表)
    - [92.反转链表2](#92反转链表2)
    - [25.K个一组翻转链表](#25k个一组翻转链表)
    - [83.删除排序链表中的重复元素](#83删除排序链表中的重复元素)
    - [82.删除排序链表中的重复元素2](#82删除排序链表中的重复元素2)
    - [19.删除链表的倒数第N个节点](#19删除链表的倒数第n个节点)
    - [86.分隔链表](#86分隔链表)
    - [61.旋转链表](#61旋转链表)
    - [143.重排链表](#143重排链表)
    - [24.两两交换链表中的节点](#24两两交换链表中的节点)
    - [141.环形链表](#141环形链表)
    - [142.环形链表2](#142环形链表2)
    - [876.链表的中间节点](#876链表的中间节点)
    - [剑指 Offer 52. 两个链表的第一个公共节点](#剑指-offer-52-两个链表的第一个公共节点)
    - [146.LRU缓存机制](#146lru缓存机制)
    - [460. LFU 缓存](#460-lfu-缓存)
    - [138.复制带随机指针的链表](#138复制带随机指针的链表)
    - [234. 回文链表](#234-回文链表)


### 二.链表专题


---------------------------
##### 21.合并两个有序链表
>题目描述:将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-two-sorted-lists

解题思路：双指针法创建。

时间复杂度：O(M+N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        while (l1 && l2)
        {
            if (l1->val <= l2->val)
            {
                tail->next = l1;
                tail = l1;
                l1 = l1->next;
            }
            else 
            {
                tail->next = l2;
                tail = l2;
                l2 = l2->next;
            }
        }
        if (l1)
            tail->next = l1;
        if (l2)
            tail->next = l2;
        return dummy.next;
    }
};

```

<br>


---------------------------
##### 23.合并K个升序链表
>题目描述:给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-k-sorted-lists

* **解法一**

解题思路：用一个for循环，将链表数组中的所有链表都依次按照 两个升序链表合并 的方法进行合并。

时间复杂度：O(N + 2N + 3N +  .. + KN) ~= O(K^2 * N) N为链表的平均长度，K为链表的个数

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2)
    {
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        while (l1 && l2)
        {
            if (l1->val <= l2->val)
            {
                tail->next = l1;
                tail = l1;
                l1 = l1->next;
            }
            else 
            {
                tail->next = l2;
                tail = l2;
                l2 = l2->next;
            }
        }
        if (l1)
            tail->next = l1;
        if (l2)
            tail->next = l2;
        return dummy.next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* ans = nullptr;
        for (int i = 0; i < lists.size(); ++i)
            ans = merge(lists[i], ans);
        return ans;
    }
};

```


* **解法二**

解题思路：在解法一中进行优化，不断的两两归并链表直到只剩下最后一个链表。实质上也就是归并排序的链表处理法。

时间复杂度：O((k/2*2N + k/4*4N + ... ) * logK) ~= O(KN*logK) N为链表的平均长度， K为链表的个数

空间复杂度：O(logK) 

```cpp
 class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2)
    {
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        while (l1 && l2)
        {
            if (l1->val <= l2->val)
            {
                tail->next = l1;
                tail = l1;
                l1 = l1->next;
            }
            else 
            {
                tail->next = l2;
                tail = l2;
                l2 = l2->next;
            }
        }
        if (l1)
            tail->next = l1;
        if (l2)
            tail->next = l2;
        return dummy.next;
    }

    ListNode* mergeSort(vector<ListNode*>& lists, int l, int r)
    {
        if (l == r)
            return lists[l];
        else if (l > r)
            return nullptr;
        else
        {
            int mid = (l + r) >> 1;
            ListNode* left = mergeSort(lists, l, mid);
            ListNode* right = mergeSort(lists, mid+1, r);
            return merge(left, right);
        }
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty())
            return nullptr;
        int l = 0, r = lists.size()-1;
        return mergeSort(lists, l, r);
    }
};

```


<br>


---------------------------
##### 147.对链表进行插入排序
>题目描述:对链表进行插入排序。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/insertion-sort-list


* **解法一**

解题思路：插入排序的链表版

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        ListNode dummy(-1);
        ListNode* p = head;
        while (p)
        {
            ListNode* tmp = p->next;
            
            ListNode* q = &dummy;
            while (q->next && q->next->val <= p->val)
                q = q->next;
            p->next = q->next;
            q->next = p;
            
            p = tmp;
        }
        return dummy.next;
    }
};

```



* **解法二**

解题思路：冒泡排序，不过这里的排序只交换了数值，没有交换节点。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        ListNode *end = nullptr;

        while (head != end)
        {
            ListNode* p = head;

            while (p->next != end)
            {
                if (p->val > p->next->val)
                    swap(p->val, p->next->val);
                p = p->next;
            }

            end = p;
        }

        return head;
    }
};

```


<br>




<br>

---------------------------
##### 148.排序链表
>题目描述:给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。
进阶：
你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sort-list

* **解法一**

解题思路：归并排序，需要通过快慢指针找到节点的中间位置。

时间复杂度：O(NlogN)

空间复杂度：O(logN)

```cpp
class Solution {
public:
    ListNode* findMid(ListNode* head)
    {
        ListNode* s = head, *f = head;
        while(1)
        {
            if (!f || !f->next || !f->next->next)
                return s;
            s = s->next;
            f = f->next->next;
        }
        return s;
    }    

    ListNode* merge(ListNode* l1, ListNode* l2)
    {
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        while (l1 && l2)
        {
            if (l1->val <= l2->val)
            {
                tail->next = l1;
                tail = l1;
                l1 = l1->next;
            }else
            {
                tail->next = l2;
                tail = l2;
                l2 = l2->next;
            }
        }
        if (l1)
            tail->next = l1;
        if (l2)
            tail->next = l2;
        return dummy.next;
    }

    ListNode* mergeSort(ListNode* head)
    {
        if (!head || !head->next)
            return head;
        ListNode* mid = findMid(head);
        ListNode* l2 = mergeSort(mid->next);
        mid->next = nullptr;
        ListNode* l1 = mergeSort(head);
        return merge(l1, l2);
    }

    ListNode* sortList(ListNode* head) {
        return mergeSort(head);
    }
};
```

* **解法二**

解题思路：快速排序，找parition分割点时，需要用两个临时节点连接左右链表，左边的链表比key值小，右边的链表比key值大。在这道题上面用快速排序会超时。

时间复杂度：O(NlogN)

空间复杂度：O(logN)

```cpp
class Solution {
public:
    ListNode* quickSort(ListNode* head)
    {
        if (!head)
            return nullptr;
        ListNode ldummy(-1), rdummy(-1);
        ListNode* ltail = &ldummy, *rtail = &rdummy;
        for (auto cur = head; cur; cur = cur->next)
        {
            if (cur->val < head->val)
            {
                ltail->next = cur;
                ltail = cur;
            }else
            {
                rtail->next = cur;
                rtail = cur;
            }
        }
        ltail->next = nullptr, rtail->next = nullptr;

        ListNode* newRight = quickSort(head->next);
        head->next = newRight;
        ListNode* newLeft = quickSort(ldummy.next);

        ListNode dummy(-1);
        dummy.next = newLeft;
        ListNode* tail = &dummy;
        while (tail->next)
            tail = tail->next;
         tail->next = head;
         return dummy.next;       
    }

    ListNode* sortList(ListNode* head) {
        if (!head)
            return  nullptr;
        return quickSort(head);
    }
};  

```

<br>

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
            ListNode* tmp = cur->next;

            pair<ListNode*,ListNode*> newPair = reverseList(pre->next, cur);
            ListNode* newHead = newPair.first, *newTail = newPair.second;
            pre->next = newHead;
            pre = newTail;

            newTail->next = tmp;
        }
        return dummy.next;
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

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/


```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* p = head;
        while (p)
        {
            ListNode* q = p->next;
            while (q && q->val == p->val)
            {
                ListNode* t = q; // 删除节点
                q = q->next;
                delete t;      //删除节点
            }
            p->next = q;
            p = q;
        }
        return head;
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
                {
                    ListNode* t = f; //删除节点
                    f = f->next;
                    delete t; //删除节点
                }
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
        ListNode dummy(-1);
        dummy.next = head;
        ListNode *s = &dummy, *f = &dummy;
        for (int i = 0; i < n; ++i)
            f = f->next;
        while (f->next)
        {
            s = s->next;
            f = f->next;
        }
        ListNode* t =  s->next;
        s->next = t->next;
        delete t;
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
        ListNode ldummy(-1), rdummy(-1);
        ListNode *ltail = &ldummy, *rtail = &rdummy;
        for (auto p = head; p; p = p->next)
        {
            if (p->val < x)
            {
                ltail->next = p;
                ltail = p;
            }else
            {
                rtail->next = p;
                rtail = p;
            }
        }   
        ltail->next = nullptr, rtail->next = nullptr;
        ltail->next = rdummy.next;
        return ldummy.next;
    }
};

```

<br>

-----------------------------------
##### 61.旋转链表
>题目描述：给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。k可以大于链表长度
示例 1:
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-list

解题思路：先遍历整个链表将链表的长度len测量出来，并将链表的首尾相连；接着走len - k步再断开，即可求得新的链表

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next)
            return head;
        ListNode *cur = head;
        int len = 1;
        while (cur->next)
        {
            len++;
            cur = cur->next;
        }
        cur->next = head; //首尾相连
        k %= len;

        for (int i = 0; i < len - k; ++i)
            cur = cur->next;
        ListNode* newHead = cur->next;
        cur->next = nullptr;
        return newHead;
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


解题思路：只进行一次遍历即可，不断往后迭代。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode dummy(-1);
        dummy.next = head;
        ListNode *pre = &dummy;
        while (pre && pre->next && pre->next->next)
        {
            ListNode* p = pre->next, *q = pre->next->next;
            p->next = q->next, pre->next = q, q->next = p;            
            pre = p;
        }
        return dummy.next;
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
    std::list<std::pair<int,int>> list; //<key,value>
    std::unordered_map<int, std::list<std::pair<int,int>>::iterator> hashmap; //key, 对应list所在pos
    int _capacity;
public:
    LRUCache(int capacity) {
        _capacity = capacity;
    }
    
    int get(int key) {
        if (hashmap.find(key) == hashmap.end())
            return -1;
        else
        {
            auto it = hashmap[key];
            int value = it->second;
            list.splice(list.end(), list, it);
            return value;
        }
    }
    
    void put(int key, int value) {
        if (hashmap.find(key) != hashmap.end())
        {
            auto it = hashmap[key];
            it->second = value;
            list.splice(list.end(), list, it);
        }else
        {
            if (_capacity == list.size())
            {
                hashmap.erase(list.front().first);
                list.pop_front();
            }
            list.push_back(std::make_pair(key, value));
            hashmap[key] = std::prev(list.end());            
        }
    }
};


```

<br>


-----------------------------------
##### 460. LFU 缓存
>题目描述：请你为 最不经常使用（LFU）缓存算法设计并实现数据结构。
实现 LFUCache 类：
LFUCache(int capacity) - 用数据结构的容量 capacity 初始化对象
int get(int key) - 如果键存在于缓存中，则获取键的值，否则返回 -1。
void put(int key, int value) - 如果键已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量时，则应该在插入新项之前，使最不经常使用的项无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 最近最久未使用 的键。
注意「项的使用次数」就是自插入该项以来对其调用 get 和 put 函数的次数之和。使用次数会在对应项被移除后置为 0 。
为了确定最不常使用的键，可以为缓存中的每个键维护一个 使用计数器 。使用计数最小的键是最久未使用的键。
当一个键首次插入到缓存中时，它的使用计数器被设置为 1 (由于 put 操作)。对缓存中的键执行 get 或 put 操作，使用计数器的值将会递增。
进阶：你可以为这两种操作设计时间复杂度为 O(1) 的实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lfu-cache

解题思路：设置两个哈希表，分别是key哈希和freq哈希。key哈希可以快速查找有无该key值；freq哈希可以记录不同频率的节点，
list左边代表新加入的，右边代表后加入的，容量满的话删除右边的节点。同时设置一个最小频率，作为记录。这样设计可以保证该LFU算法的时间复杂度为O(1)。

时间复杂度：O(1)

空间复杂度：O(N)


```cpp
struct Node{
    int key;
    int val;
    int freq;
    Node(int _key, int _val, int _freq) : key(_key), val(_val), freq(_freq){}
};


class LFUCache {
    int _minFreq; //当前最小频率
    int _capacity; 
    std::unordered_map<int, std::list<Node>::iterator> _keyTable;
    std::unordered_map<int, list<Node>> _freqTable;
public:
    LFUCache(int capacity) {
        _minFreq = 0;
        _capacity = capacity;
    }
    
    int get(int key) {
        if (_capacity == 0 || _keyTable.find(key) == _keyTable.end())
            return -1;
        std::list<Node>::iterator it = _keyTable[key];
        int val = it->val, freq = it->freq;
        //删除freq链表记录
        _freqTable[freq].erase(it);
        //若链表为空，那么删除该freq对应链表并更新_minFreq；
        if (_freqTable[freq].empty())
        {
            _freqTable.erase(freq);
            if (_minFreq == freq)
                _minFreq += 1;
        }
        //频率+1，并重新插入链表
        _freqTable[freq + 1].push_front(Node(key, val, freq+1));
        _keyTable[key] = _freqTable[freq + 1].begin();
        return val;
    }
    
    void put(int key, int value) {
        if (_capacity == 0)
            return;
        //未找到该值，进行添加
        if (_keyTable.find(key) == _keyTable.end())
        {
            if (_keyTable.size() == _capacity)
            {
                //容量已满，需要将最后的节点删除
                auto node = _freqTable[_minFreq].back();
                _keyTable.erase(node.key);
                _freqTable[_minFreq].pop_back();
                if (_freqTable[_minFreq].empty())
                {
                    _freqTable.erase(_minFreq);
                }
            }
            //有空余位置，直接添加
            _freqTable[1].push_front(Node(key, value, 1));
            _keyTable[key] = _freqTable[1].begin();
            _minFreq = 1;
        }else//找到了该value，类似 get操作
        {
            std::list<Node>::iterator it = _keyTable[key];
            int freq = it->freq;
            _freqTable[freq].erase(it);
            if (_freqTable[freq].empty())
            {
                _freqTable.erase(freq);
                if (_minFreq == freq)
                    _minFreq += 1;
            }
            _freqTable[freq + 1].push_front(Node(key, value, freq+1));
            _keyTable[key] = _freqTable[freq + 1].begin();
        }
        
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
        for(auto cur = head; cur; cur = cur->next->next)
        {
            Node* node = new Node(cur->val);
            node->next = cur->next;
            cur->next = node;
        }
        for (auto cur = head; cur; cur = cur->next->next)
            cur->next->random = cur->random ? cur->random->next :nullptr;
        Node dummy(-1);
        Node *tail = &dummy;
        for (auto cur = head; cur; cur = cur->next)
        {
            Node* node = cur->next;
            cur->next = cur->next->next;
            tail->next = node;
            tail = node;    
        }
        tail->next = nullptr;
        return dummy.next;
    }
};
```

<br>



-----------------------------------
##### 234. 回文链表
>题目描述：请判断一个链表是否为回文链表。
进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-linked-list

解题思路：先找到中间节点，再将中间节点以后的链表进行反转，再判断这两个链表是否相等

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
            if (!f || !f->next || !f->next->next)
                return s;
            s = s->next;
            f = f->next->next;
        }
        return s;
    }
    
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

    bool check(ListNode* l1, ListNode* l2)
    {
        while  (l1 && l2)
        {
            if (l1->val != l2->val)
                return false;
            l1 = l1->next;
            l2 = l2->next;
        }
        return true;
    }

    bool isPalindrome(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode* mid = middleNode(head);
        ListNode* rHead = reverseList(mid->next);
        mid->next = nullptr;;
        return check(head, rHead);        
    }

};


```


<br>
