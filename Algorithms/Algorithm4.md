- [四.树专题](#四树专题)
        - [144.二叉树的前序遍历](#144二叉树的前序遍历)
        - [94.二叉树的中序遍历](#94二叉树的中序遍历)
        - [145.二叉树的后序遍历](#145二叉树的后序遍历)
        - [102.二叉树的层序遍历](#102二叉树的层序遍历)
        - [107.二叉树的层次遍历2](#107二叉树的层次遍历2)
        - [103.二叉树的锯齿形层次遍历](#103二叉树的锯齿形层次遍历)
        - [99.恢复二叉搜索树](#99恢复二叉搜索树)
        - [100.相同的树](#100相同的树)
        - [101.对称二叉树](#101对称二叉树)
        - [110.平衡二叉树](#110平衡二叉树)
        - [114.二叉树展开为链表](#114二叉树展开为链表)
        - [116.充填每个节点的下一个右侧节点指针](#116充填每个节点的下一个右侧节点指针)
        - [117.充填每个节点的下一个右侧节点指针2](#117充填每个节点的下一个右侧节点指针2)
        - [105.从前序与中序遍历序列构造二叉树](#105从前序与中序遍历序列构造二叉树)
        - [106.从中序与后序遍历序列构造二叉树](#106从中序与后序遍历序列构造二叉树)
        - [96.不同的二叉搜索树](#96不同的二叉搜索树)
        - [95.不同的二叉搜索树2](#95不同的二叉搜索树2)
        - [98.验证二叉搜索树](#98验证二叉搜索树)
        - [108.将有序数组转换为二叉搜索树](#108将有序数组转换为二叉搜索树)
        - [109.有序链表转换二叉搜索树](#109有序链表转换二叉搜索树)
        - [111.二叉树的最小深度](#111二叉树的最小深度)
        - [104.二叉树的最大深度](#104二叉树的最大深度)
        - [112.路径总和](#112路径总和)
        - [113.路径总合2](#113路径总合2)
        - [124.二叉树中的最大路径和](#124二叉树中的最大路径和)
        - [129.求根到叶子节点数字之和](#129求根到叶子节点数字之和)



# 四.树专题

---------------------------
##### 144.二叉树的前序遍历
>题目描述:
给你二叉树的根节点 root ，返回它节点值的 前序 遍历。进阶：递归算法很简单，你可以通过迭代算法完成吗？
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-preorder-traversal

解题思路：用辅助栈来模拟递归，顺序为 根弹栈->右子树入栈->左子树入栈，在弹栈时执行打印操作。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root)
            return {};
        vector<int> ans;
        std::stack<TreeNode*> stk;
        stk.push(root);
        while (!stk.empty())
        {
            TreeNode* node = stk.top();
            stk.pop();
            ans.push_back(node->val);
            if (node->right) stk.push(node->right);
            if (node->left) stk.push(node->left);
        }
        return ans;
    }
};

```

<br>


---------------------------
##### 94.二叉树的中序遍历
>题目描述：给定一个二叉树的根节点 root ，返回它的 中序 遍历。要求使用非递归法。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-inorder-traversal

解题思路：中序遍历的方法与 前序遍历，后续遍历不同，中序遍历在一开始需要从根节点开始，一直向左子树移动并入栈， 然后 弹栈->进入右子树->不断向左子树移动并入栈  执行该操作，直到栈为空才停止。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        std::stack<TreeNode*> stk;
        while  (root)
        {
            stk.push(root);
            root = root->left;
        }
        while (!stk.empty())
        {
            TreeNode* node = stk.top();
            stk.pop();
            ans.push_back(node->val);
            
            node = node->right;
            while(node)
            {
                stk.push(node);
                node = node->left;
            }
        }
        return ans; 
    }
};

```

<br>


---------------------------
##### 145.二叉树的后序遍历
>题目描述:给定一个二叉树，返回它的 后序 遍历。
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-postorder-traversal

解题思路：该题的解题方法可以直接套用 二叉树的前序遍历， 前序遍历的结构为 根左右，后序遍历的结构为 左右根，那么我们可以先求 根右左，再翻转即可。求 根右左 的方法就跟 前序遍历 的方法类似了。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if (!root)
            return {};
        vector<int> ans;
        std::stack<TreeNode*> stk;
        stk.push(root);
        while (!stk.empty())
        {
            TreeNode* node = stk.top();
            stk.pop();
            ans.push_back(node->val);
            if (node->left) stk.push(node->left);
            if (node->right) stk.push(node->right);
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};

```

<br>


---------------------------
##### 102.二叉树的层序遍历
>题目描述:给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal

解题思路：调用辅助队列，每一次都将队列中的节点出队并将其左右子树重新入队，直到队列为空

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root)
            return {};
        std::queue<TreeNode*> que;
        vector<vector<int>> ans;
        que.push(root);
        while (!que.empty())
        {
            vector<int> vec;
            std::queue<TreeNode*> tempQue;
            while (!que.empty())
            {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) tempQue.push(node->left);
                if (node->right) tempQue.push(node->right);
            }
            swap(tempQue, que);
            ans.push_back(std::move(vec));
        }  
        return ans;
    }
};


```

<br>

---------------------------
##### 107.二叉树的层次遍历2
>题目描述:给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii

解题思路：该题是 二叉树的层序遍历 变形题，只需要将 层序遍历的结果进行一次翻转即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        if (!root)
            return {};
        std::queue<TreeNode*> que;
        vector<vector<int>> ans;
        que.push(root);
        while (!que.empty())
        {
            vector<int> vec;
            std::queue<TreeNode*> tempQue;
            while (!que.empty())
            {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) tempQue.push(node->left);
                if (node->right) tempQue.push(node->right);
            }
            swap(tempQue, que);
            ans.push_back(std::move(vec));
        }
        reverse(ans.begin(), ans.end());  
        return ans;
    }
};

```

<br>

---------------------------
##### 103.二叉树的锯齿形层次遍历
>题目描述:给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal

解题思路：该题是 二叉树的层序遍历 变形题，只需要每一次重新入队完毕，再各一次进行一次翻转即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (!root)
            return {};
        int cnt = 0;
        std::queue<TreeNode*> que;
        vector<vector<int>> ans;
        que.push(root);
        while (!que.empty())
        {
            vector<int> vec;
            std::queue<TreeNode*> tempQue;
            while (!que.empty())
            {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) tempQue.push(node->left);
                if (node->right) tempQue.push(node->right);
            }
            swap(tempQue, que);
            if (cnt & 0x1)
                reverse(vec.begin(), vec.end());
            ans.push_back(std::move(vec));
            cnt++;
        }
        return ans;
    }
};

```

<br>

---------------------------
##### 99.恢复二叉搜索树
>题目描述:给你二叉搜索树的根节点 root ，该树中的两个节点被错误地交换。请在不改变其结构的情况下，恢复这棵树。
进阶：使用 O(n) 空间复杂度的解法很容易实现。你能想出一个只使用常数空间的解决方案吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/recover-binary-search-tree

* **解法一**

解题思路：暴力法，直接进行中序遍历存放在数组中，该题目就变成了 在递增数组中寻找交换位置的两个数 的问题了。找到这两个数之后，再次中序遍历树，标记好节点进行交换。 
如何在递增数组中找到交换位置的两个数？找出升序数组中的交换位置（假设为x与y）：只需要进行一次遍历，如果只有一个逆序对，那么位置x在前，y在后；假设有两个逆序对，那么对y进行修改在后面的那个即可。
 
时间复杂度：O(N)

空间复杂度：O(N)


* **解法二**

解题思路：在暴力法的基础上进行改进，不用建立数组，而是在中序遍历的过程中直接用两个指针进行指向即可。遍历过程中仍然是可能会出现一个逆序对或者两个逆序对的情况。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    TreeNode* x= nullptr;
    TreeNode* y = nullptr;
    TreeNode* pre = nullptr;
public:
    void DFS(TreeNode* root)
    {
        if (!root)
            return;
        DFS(root->left);
        if (!pre)
            pre = root;
        else
        {
            if (pre->val > root->val)
            {
                y = root;
                if (!x)
                    x = pre;
            }
            pre = root;
        }
        DFS(root->right);
    }

    void recoverTree(TreeNode* root) {
        if (!root)
            return;
        DFS(root);
        swap(x->val, y->val);
    }
};

```


<br>

---------------------------
##### 100.相同的树
>题目描述:给定两个二叉树，编写一个函数来检验它们是否相同。
如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/same-tree

解题思路： 两个二叉树同时进行前序遍历，判断 根 && 左 && 右 是否相同。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q)
            return true;
        else if (!p || !q)
            return false;
        else
            return p->val==q->val && isSameTree(p->left,q->left) && isSameTree(p->right, q->right);
    }
};
```

<br>

---------------------------
##### 101.对称二叉树
>题目描述:给定一个二叉树，检查它是否是镜像对称的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/symmetric-tree

解题思路：递归法，判断A树与B树是否为对称，也就是需要比较A->val==B->val， A->left==B->right， A->right==B->left 判断是否相等即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* A, TreeNode* B)
    {
        if (!A && !B)
            return true;
        else if (!A || !B)
            return false;
        else
            return A->val==B->val && isSymmetric(A->left, B->right) && isSymmetric(A->right, B->left);
    }

    bool isSymmetric(TreeNode* root) {
        if (!root)
            return true;
        return isSymmetric(root->left, root->right);
    }
};

```

<br>

---------------------------
##### 110.平衡二叉树
>题目描述:给定一个二叉树，判断它是否是高度平衡的二叉树。
本题中，一棵高度平衡二叉树定义为：
一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/balanced-binary-tree

解题思路：后序遍历的方法，递归函数返回的是树的高度，分别得到左子树与右子树的高度，并计算差的绝对值判断是否平衡。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    bool ans = true;
public:
    int DFS(TreeNode* root)
    {
        if (!root)
            return 0;
        int l = DFS(root->left);
        int r = DFS(root->right);
        if (abs(l-r)>1)
            ans = false;
        return std::max(l, r) + 1;
    }

    bool isBalanced(TreeNode* root) {
        if (!root)
            return true;
        DFS(root);
        return ans;
    }
};

```

<br>

---------------------------
##### 114.二叉树展开为链表
>题目描述:给定一个二叉树，原地将它展开为一个单链表。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list

解题思路：

时间复杂度：

空间复杂度：

```cpp
19:05

```

<br>

---------------------------
##### 116.充填每个节点的下一个右侧节点指针
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 117.充填每个节点的下一个右侧节点指针2
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 105.从前序与中序遍历序列构造二叉树
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 106.从中序与后序遍历序列构造二叉树
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 96.不同的二叉搜索树
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 95.不同的二叉搜索树2
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 98.验证二叉搜索树
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 108.将有序数组转换为二叉搜索树
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 109.有序链表转换二叉搜索树
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 111.二叉树的最小深度
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 104.二叉树的最大深度
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 112.路径总和
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 113.路径总合2
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 124.二叉树中的最大路径和
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

---------------------------
##### 129.求根到叶子节点数字之和
>题目描述:

解题思路：

时间复杂度：

空间复杂度：

```cpp


```

<br>

