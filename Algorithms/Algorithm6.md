- [六.排序专题](#六排序专题)
        - [88.合并两个有序数组](#88合并两个有序数组)
        - [21.合并两个有序链表](#21合并两个有序链表)
        - [23.合并K个升序链表](#23合并k个升序链表)
        - [147.对链表进行插入排序](#147对链表进行插入排序)
        - [148.排序链表](#148排序链表)
        - [75.颜色分类](#75颜色分类)
        - [剑指 Offer 40. 最小的k个数](#剑指-offer-40-最小的k个数)
        - [剑指 Offer 41. 数据流中的中位数](#剑指-offer-41-数据流中的中位数)
        - [剑指 Offer 45. 把数组排成最小的数](#剑指-offer-45-把数组排成最小的数)
        - [剑指 Offer 51. 数组中的逆序对](#剑指-offer-51-数组中的逆序对)



# 六.排序专题

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



* **解法二**

解题思路：冒泡排序，不过这里的排序只交换了数值，没有交换节点。

时间复杂度：O(N^2)

空间复杂度：O(1)

```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode* end = nullptr;
        while (head->next != end)
        {
            ListNode* cur = head;
            while (cur->next != end)
            {
                if (cur->val > cur->next->val)
                    swap(cur->val, cur->next->val);
                cur = cur->next;
            }
            end = cur;
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

解题思路：快速排序，找parition分割点时，需要用两个临时节点连接左右链表，左边的链表比key值小，右边的链表比key值大。

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

---------------------------
##### 75.颜色分类
>题目描述:给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。
进阶：
你可以不使用代码库中的排序函数来解决这道题吗？
你能想出一个仅使用常数空间的一趟扫描算法吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sort-colors

* **解法一**

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

*  **解法二**

解题思路：设置三个指针，p0指针代表0的范围，p2指针代表2的范围，用for循环开始遍历i即可 ，注意i不可能比p0走的慢，交换2时i是不会增加的。

时间复杂度：O(N)

空间复杂度：O(1)


```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int p0 = -1, p2 = nums.size(), i = 0;
        while (i < p2)
        {
            if  (1 == nums[i])
                ++i;
            else if (0 == nums[i])
                swap(nums[++p0], nums[i++]);
            else //2
                swap(nums[--p2], nums[i]);
        }
    }
};

```

<br>



---------------------------
##### 剑指 Offer 40. 最小的k个数
>题目描述:输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof

* **解法一**

解题思路：该题为经典的topk算法题，解法一使用快速排序法。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    int partition(vector<int>& nums, int l, int r)
    {
        int key = nums[l];
        int i = l, j = r;
        while (i < j)
        {
            while (i<j && nums[j]>=key) j--;
            if (i<j) nums[i++] = nums[j];
            while (i<j && nums[i]<key) i++;
            if (i<j) nums[j--] = nums[i]; 
        }
        nums[i] = key;
        return i;
    }

    vector<int> quickSort(vector<int>& nums, int k)
    {
        int l = 0, r = nums.size()-1;
        while (l <= r)
        {
            int mid = partition(nums, l, r);
            if (mid == k-1)
                return vector<int>(nums.begin(), nums.begin()+k);
            else if (mid < k-1)
                l = mid + 1;
            else 
                r = mid - 1;
        }   
        return {};
    }

    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if (k<=0 || k>arr.size())
            return {};
        return quickSort(arr, k);
    }
};

```

* **解法二**

解题思路：该题为经典的topk算法题，解法二使用原地的堆排序法。

时间复杂度：O(NlogK)

空间复杂度：O(1)

```cpp
class Solution {
public:
    void heapAdjust(vector<int>& nums, int beg, int end)
    {
        int dad = beg, son = 2 * dad + 1;
        while (son <= end)
        {
            if (son+1<=end && nums[son+1]>=nums[son]) son = son+1;
            if (nums[dad] >= nums[son]) break;
            swap(nums[dad], nums[son]);
            dad = son;
            son = dad * 2 + 1;
        }
    }

    vector<int> heapSort(vector<int>& nums, int pos)
    {
        //先创建大小为k的最大堆
        for (int i = (pos-1)/2; i >= 0; i--)
            heapAdjust(nums, i, pos);
        //创建好最大堆之后，将数组后面的元素依次和堆顶进行比较
        for (int i = pos+1; i<nums.size(); i++)
            if (nums[i] < nums[0])
            {
                swap(nums[0], nums[i]);
                heapAdjust(nums, 0, pos);
            }
        return vector<int>(nums.begin(), nums.begin()+pos+1);
    }

    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if (k<=0 || k>arr.size())
            return {};
        return heapSort(arr, k-1);        
    }
};

```

* **解法三**

解题思路：该题为经典的topk算法题，解法三使用STL库中的堆法。

时间复杂度：O(NlogK)

空间复杂度：O(K)

```cpp
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if (k<=0 || k>arr.size())
            return {};
        std::priority_queue<int, vector<int>, std::less<int>> heap; //最大堆
        for (int i = 0; i < k; i++)
            heap.push(arr[i]);
        for (int i = k; i < arr.size(); i++)
            if (arr[i] < heap.top())
            {
                heap.push(arr[i]);
                heap.pop();
            }
        vector<int> ans;
        while(!heap.empty())
        {
            ans.push_back(heap.top());
            heap.pop();
        }
        return ans;
    }
};

```

<br>

---------------------------
##### 剑指 Offer 41. 数据流中的中位数
>题目描述:如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
例如，
[2,3,4] 的中位数是 3
[2,3] 的中位数是 (2 + 3) / 2 = 2.5
设计一个支持以下两种操作的数据结构：
void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof

解题思路：设置两个堆，左边为最大堆，右边为最小堆，且保证右边的堆始终比左边的堆大；这也就是说，如果要加入一个数，得先进左边堆，再进右边的堆。

时间复杂度：addNum:O(logN) findMedian:O(1)

空间复杂度：O(N)

```cpp
class MedianFinder {
    std::priority_queue<int, vector<int>, std::less<int>> lheap;//左边的大顶堆
    std::priority_queue<int, vector<int>, std::greater<int>> rheap;//右边的小顶堆
public:
    /** initialize your data structure here. */
    MedianFinder() {
    }
    
    void addNum(int num) {
        lheap.push(num);
        rheap.push(lheap.top());
        lheap.pop();
        if (lheap.size() < rheap.size())
        {
            lheap.push(rheap.top());
            rheap.pop();
        }
    }
    
    double findMedian() {
        int n = lheap.size() + rheap.size();
        if (n & 0x1)
            return lheap.top();
        else
            return (double)(lheap.top()+rheap.top())/2;
    }
};

```

<br>



---------------------------
##### 剑指 Offer 45. 把数组排成最小的数
>题目描述:输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
输出结果可能非常大，所以你需要返回一个字符串而不是整数
拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof

解题思路：自定义排序

时间复杂度：O(NlogN)

空间复杂度：O(1)

```cpp
class Solution {
public:
    string minNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end(), [&](int lhs, int rhs){
            return to_string(lhs)+to_string(rhs) < to_string(rhs)+to_string(lhs);
        });
        string ans;
        for (auto& e: nums)
            ans += to_string(e);
        return ans;
    }
};

```

<br>


---------------------------
##### 剑指 Offer 51. 数组中的逆序对
>题目描述:在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof

解题思路：归并排序，在进行归并的过程中顺势可以计算逆序对的数量。

时间复杂度：O(NlogN)

空间复杂度：O(logN)

```cpp
class Solution {
    int ans = 0;
public:
    void merge(vector<int>& nums, int l, int mid, int r, vector<int>& temp)
    {
        int i = l, j = mid+1, k = l;
        while (i<=mid && j<=r)
        {
            if (nums[i] <= nums[j])
                temp[k++] = nums[i++];
            else
            {
                //逆序对出现
                ans += mid - i + 1;
                temp[k++] = nums[j++];
            }
        }
        while (i <= mid)
            temp[k++] = nums[i++];
        while (j <= r)
            temp[k++] = nums[j++];
        for (int k = l; k <= r; ++k)
            nums[k] = temp[k];
    }

    void mergeSort(vector<int>& nums, int l, int r, vector<int>& temp)
    {
        if (l < r)
        {
            int mid = (l + r) >> 1;
            mergeSort(nums, l, mid, temp);
            mergeSort(nums, mid+1, r, temp);
            merge(nums, l, mid, r, temp);
        }
    }

    int reversePairs(vector<int>& nums) {
        if (nums.empty())
            return 0;
        vector<int> temp(nums.size());
        mergeSort(nums, 0, nums.size()-1, temp);
        return ans;
    }
};

```

<br>
