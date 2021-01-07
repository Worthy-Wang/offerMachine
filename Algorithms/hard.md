

# leetcode精选高频hard题目


--------------------------
##### 4.寻找两个正序数组的中位数
>题目描述：给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。
要求：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/median-of-two-sorted-arrays

解题思路：该题用双指针法求解，不断从双指针中较小的数往后推进，并且中位数k不断二分减小，这样可以让双指针快速往后推进，达到二分效果。

时间复杂度：O(log(M+N))

空间复杂度：O(1)


```cpp
class Solution {
public:
    double findKth(vector<int>& nums1, vector<int>& nums2, int k)
    {
        int i = 0, j = 0;
        int len1 = nums1.size(), len2 = nums2.size();
        while (1)
        {
            if (i == len1)
                return nums2[j + k - 1];
            if (j == len2)
                return nums1[i + k - 1];
            if (1 == k)
                return std::min(nums1[i], nums2[j]);

            int newi = std::min(i+k/2-1, len1-1);
            int newj = std::min(j+k/2-1, len2-1);
            if (nums1[newi] < nums2[newj])
            {
                k -= newi + 1 - i;
                i = newi + 1;
            }
            else
            {
                k -= newj + 1 - j;
                j = newj + 1;
            }
        }
        return 0;
    }


    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int len = nums1.size() + nums2.size();
        if (len & 0x1)//奇数
            return findKth(nums1, nums2, len/2+1);
        else 
            return (findKth(nums1, nums2, len/2) + findKth(nums1, nums2, len/2+1))/2;        
    }
};

```

<br>




