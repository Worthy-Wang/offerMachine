- [五.排序专题](#五排序专题)
        - [88.合并两个有序数组](#88合并两个有序数组)
        - [21.合并两个有序链表](#21合并两个有序链表)
        - [23.合并K个升序链表](#23合并k个升序链表)
        - [147.对链表进行插入排序](#147对链表进行插入排序)
        - [148.排序链表](#148排序链表)
        - [75.颜色分类](#75颜色分类)



# 五.排序专题

---------------------------
##### 88.合并两个有序数组
>题目描述:
给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。
说明：
初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-sorted-array

解题思路：双指针，两个指针从后往前移动即可。

时间复杂度：O(M+N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m-1, j = n-1, k = m+n-1;
        while (0<=i && 0<=j)
        {
            if (nums1[i] >= nums2[j])
                nums1[k--] = nums1[i--];
            else
                nums1[k--] = nums2[j--];
        }
        while (0 <= i)
            nums1[k--] = nums1[i--];
        while (0 <= j)
            nums1[k--] = nums2[j--];
    }
};

```

<br>


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


* **解法二**

解题思路：在解法一中进行优化，不断的两两归并链表直到只剩下最后一个链表。实质上也就是归并排序的链表处理法。

时间复杂度：O(K*N*logK) N为链表的平均长度， K为链表的个数

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

解题思路：插入排序的链表版

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        ListNode dummy(-1);
        ListNode* cur = head;
        while (cur)
        {
            ListNode* t = cur->next;

            ListNode *p = &dummy;
            while (p->next)
            {
                if (p->next->val >= cur->val)
                    break;
                p = p->next;
            }
            cur->next = p->next;
            p->next = cur;

            cur = t;
        }
        return dummy.next;
    }
};

```

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
        while (f)
        {
            if (!f->next || !f->next->next)
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

    ListNode* mergeSort(ListNode* head)
    {
        if (!head || !head->next)
            return head;
        ListNode* ltail = findMid(head), *rHead = ltail->next;
        ltail->next = nullptr; //左右两边断开
        ListNode* newLhead = mergeSort(head);
        ListNode* newRhead = mergeSort(rHead);
        return merge(newLhead, newRhead);
    }    

    ListNode* sortList(ListNode* head) {
        return mergeSort(head);
    }
};

```

* **解法二**

解题思路：快速排序，找parition分割点时，需要用两个临时节点连接左右链表，左边的链表比key值小，右边的链表比key值大。

时间复杂度：O(NlogN)

空间复杂度：O(logN)

```cpp
class Solution {
public:
ListNode *quickSort(ListNode *head)
{
    if (!head || !head->next)
        return head;
    int key = head->val;
    ListNode ldummy(-1), rdummy(-1);
    ListNode *ltail = &ldummy, *rtail = &rdummy; //将链表左右分割
    for (auto p = head; p; p = p->next)
    {
        if (p->val < key)
        {
            ltail->next = p;
            ltail = p;
        }
        else
        {
            rtail->next = p;
            rtail = p;
        }
    }
    ltail->next = nullptr, rtail->next = nullptr;

    ListNode *newRightHead = quickSort(head->next);
    ListNode *newleftHead = quickSort(ldummy.next);
    if (newleftHead)
    {
        auto p = newleftHead;
        while (p->next)
                p = p->next;
        p->next = head;
        head->next = newRightHead;
        return newleftHead;
    }
    else
        return newRightHead;
}

    ListNode* sortList(ListNode* head) {
        return quickSort(head);
    }
};


```


<br>


---------------------------
##### 75.颜色分类
>题目描述:给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。
进阶：
你可以不使用代码库中的排序函数来解决这道题吗？
你能想出一个仅使用常数空间的一趟扫描算法吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sort-colors


解题思路：双指针法，设置快慢指针执行两次，每一次分别在慢指针出处安置好 0, 1 这三个数。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int i = 0, j = 0;
        for (int j = 0; j < nums.size(); j++)
            if (0 == nums[j])
                swap(nums[i++], nums[j]);
        for (int j = i; j < nums.size(); j++)
            if (1 == nums[j])
                swap(nums[i++], nums[j]);
    }
};



```

<br>


