- [四.树专题](#四树专题)
        - [144.二叉树的前序遍历](#144二叉树的前序遍历)
        - [94.二叉树的中序遍历](#94二叉树的中序遍历)
        - [145.二叉树的后序遍历](#145二叉树的后序遍历)
        - [102.二叉树的层序遍历](#102二叉树的层序遍历)
        - [107.二叉树的层次遍历2](#107二叉树的层次遍历2)
        - [103.二叉树的锯齿形层次遍历](#103二叉树的锯齿形层次遍历)
        - [100.相同的树](#100相同的树)
        - [101.对称二叉树](#101对称二叉树)
        - [226. 翻转二叉树](#226-翻转二叉树)
        - [剑指 Offer 26. 树的子结构](#剑指-offer-26-树的子结构)
        - [110.平衡二叉树](#110平衡二叉树)
        - [111.二叉树的最小深度](#111二叉树的最小深度)
        - [104.二叉树的最大深度](#104二叉树的最大深度)
        - [114.二叉树展开为链表](#114二叉树展开为链表)
        - [116.充填每个节点的下一个右侧节点指针](#116充填每个节点的下一个右侧节点指针)
        - [117.充填每个节点的下一个右侧节点指针2](#117充填每个节点的下一个右侧节点指针2)
        - [105.从前序与中序遍历序列构造二叉树](#105从前序与中序遍历序列构造二叉树)
        - [106.从中序与后序遍历序列构造二叉树](#106从中序与后序遍历序列构造二叉树)
        - [297. 二叉树的序列化与反序列化](#297-二叉树的序列化与反序列化)
        - [剑指 Offer 33. 二叉搜索树的后序遍历序列](#剑指-offer-33-二叉搜索树的后序遍历序列)
        - [剑指 Offer 36. 二叉搜索树与双向链表](#剑指-offer-36-二叉搜索树与双向链表)
        - [98.验证二叉搜索树](#98验证二叉搜索树)
        - [99.恢复二叉搜索树](#99恢复二叉搜索树)
        - [96.不同的二叉搜索树](#96不同的二叉搜索树)
        - [95.不同的二叉搜索树2](#95不同的二叉搜索树2)
        - [108.将有序数组转换为二叉搜索树](#108将有序数组转换为二叉搜索树)
        - [109.有序链表转换二叉搜索树](#109有序链表转换二叉搜索树)
        - [112.路径总和](#112路径总和)
        - [113.路径总合2](#113路径总合2)
        - [437.路径总合3](#437路径总合3)
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
##### 226. 翻转二叉树
>题目描述:翻转一棵二叉树。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/invert-binary-tree

解题思路：后续遍历翻转即可

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root)
            return nullptr;
        invertTree(root->left);
        invertTree(root->right);
        swap(root->left, root->right);
        return root;
    }
};


```

<br>


------------------------------
##### 剑指 Offer 26. 树的子结构
>题目描述:输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)
B是A的子结构， 即 A中有出现和B相同的结构和节点值。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof

解题思路：两棵二叉树同时进行递归，只要能将B的所有节点匹配完成即可。

时间复杂度：O(M*N) M是A的节点数，N是B的节点数

空间复杂度：O(M) 最坏的退化成链表的情况也就是M，M一定大于N

```cpp
class Solution {
public:
    bool isSame(TreeNode* A, TreeNode* B)
    {
        if (!B) return true;
        if (!A) return false;
        return A->val==B->val && isSame(A->left, B->left) && isSame(A->right, B->right);
    }

    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (!A || !B) return false;
        return isSame(A, B) || isSubStructure(A->left, B) || isSubStructure(A->right, B);
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
##### 111.二叉树的最小深度
>题目描述:给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
说明：叶子节点是指没有子节点的节点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-depth-of-binary-tree


解题思路：DFS前序遍历找出所有的叶子节点并比较所在高度。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    int ans = INT32_MAX;
public:
    void DFS(TreeNode* root, int level)
    {
        if (!root)
            return;
        if (!root->left && !root->right)
        {
            ans = std::min(ans, level);
            return;
        }
        DFS(root->left, level+1);
        DFS(root->right, level+1);
    }

    int minDepth(TreeNode* root) {
        if (!root)
            return 0;
        DFS(root, 1);
        return ans;
    }
};

```

<br>

---------------------------
##### 104.二叉树的最大深度
>题目描述:给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-depth-of-binary-tree

解题思路：最大深度也就是高度，可以直接用后序遍历计算。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution
{
public:
    int maxDepth(TreeNode *root)
    {
        if (!root)
            return 0;
        return std::max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};

```

<br>

---------------------------
##### 114.二叉树展开为链表
>题目描述:给定一个二叉树，原地将它展开为一个单链表。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list

解题思路：1.将右子树连接到左子树的右边最后一个节点右边； 2.左子树转移到右子树 3.向右子树移动。 循环该过程

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    void flatten(TreeNode* root) {
        TreeNode* pre = nullptr, *next = nullptr, *cur = root;
        while (cur)
        {
            if (cur->left)
            {
                pre = cur->left;
                while (pre->right)
                    pre = pre->right;
                pre->right = cur->right;
                next = cur->left;
                cur->left = nullptr;
                cur->right = next;
            }   
            cur = cur->right;    
        }
        
    }
};

```

<br>

---------------------------
##### 116.充填每个节点的下一个右侧节点指针
>题目描述:给定一个 完美二叉树 ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。
初始状态下，所有 next 指针都被设置为 NULL。
进阶：
你只能使用常量级额外空间。
使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node

解题思路：利用已经创建好的next指针，不断右移，将左右子树的next指针迭代填充。每一次cur指针只负责连接left指针和right指针（如果有）的next，如果没由下一层的话，那么就直接返回。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    Node* connect(Node* root) {
        if (!root)
            return nullptr;
        Node* cur = root;
        while (cur)
        {
            Node* nextLevel = cur->left;
            if (!nextLevel)
                break;
            while (cur)
            {
                cur->left->next = cur->right;
                if (cur->next)
                    cur->right->next = cur->next->left;
                cur = cur->next;
            }
            cur = nextLevel;
        }
        return root;
    }
};

```

<br>

---------------------------
##### 117.充填每个节点的下一个右侧节点指针2
>题目描述:给定一个二叉树
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。
初始状态下，所有 next 指针都被设置为 NULL。
进阶：
你只能使用常量级额外空间。
使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii

解题思路：该题将完美二叉树改成了普通的二叉树，只需要在上一题的基础上添加更多的条件判断即可。

时间复杂度：O(N)

空间复杂度：O(1)

```cpp
class Solution {
public:
    Node* connect(Node* root) {
        if (!root)
            return nullptr;
        Node* cur = root;
        while (cur)
        {
            Node* nextLevel = nullptr, *pre = nullptr;
            while (cur)
            {
                if (cur->left)
                {
                    if (!nextLevel) nextLevel = cur->left;
                    if (!pre)
                        pre = cur->left;
                    else
                    {
                        pre->next = cur->left;
                        pre = cur->left;
                    }
                }
                if (cur->right)
                {
                    if (!nextLevel) nextLevel = cur->right;
                    if (!pre)
                        pre = cur->right;
                    else
                    {
                        pre->next = cur->right;
                        pre = cur->right;
                    }
                }
                cur = cur->next;
            }
            cur = nextLevel;
        }
        return root;
    }
};

```

<br>

------------------------------
##### 105.从前序与中序遍历序列构造二叉树
>题目描述：根据一棵树的前序遍历与中序遍历构造二叉树。
注意:
你可以假设树中没有重复的元素。
例如，给出
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：
    3
   / \
  9  20
    /  \
   15   7

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal

解题思路：前序遍历的元素存放的是根节点，代表了根节点创建的顺序；前序遍历的元素同样也可以将中序遍历分成左右子树，其中需要规定左右子树的范围，如果范围越界则说明左子树或者右子树为空。其中需要创建一个哈希表记录 前序遍历 的元素 在中序遍历 中的下标。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    TreeNode*  build(vector<int>& nums, int& i, vector<int>& inorder, int l, int r, unordered_map<int,int>& hashmap)
    {
        if (l <= r)
        {
            int value = nums[i++];
            TreeNode* node = new TreeNode(value);
            int mid = hashmap[value];
            node->left = build(nums, i, inorder, l, mid-1, hashmap);
            node->right = build(nums, i, inorder, mid+1, r, hashmap);
            return node;
        }
        else
            return nullptr;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (inorder.empty())
            return nullptr;
        unordered_map<int,int> hashmap;
        for (int i = 0; i < inorder.size(); i++)
            hashmap[inorder[i]] = i;
        int i = 0, l = 0, r = inorder.size()-1;
        return build(preorder, i, inorder, l, r, hashmap);
    }
};

```

<br>


---------------------------
##### 106.从中序与后序遍历序列构造二叉树
>题目描述:根据一棵树的中序遍历与后序遍历构造二叉树。
注意:
你可以假设树中没有重复的元素。
例如，给出
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
返回如下的二叉树：
    3
   / \
  9  20
    /  \
   15   7

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal

解题思路：该方法实质上与上一题相同，可以先将后序遍历翻转变成 根->右->左 的结构，然后仍然是将其作为 中序遍历 划分左右子树的依据，按照 根 左 右 的顺序创建即可。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    TreeNode*  build(vector<int>& nums, int& i, vector<int>& inorder, int l, int r, unordered_map<int,int>& hashmap)
    {
        if (l <= r)
        {
            int value = nums[i++];
            TreeNode* node = new TreeNode(value);
            int mid = hashmap[value];
            node->right = build(nums, i, inorder, mid+1, r, hashmap);
            node->left = build(nums, i, inorder, l, mid-1, hashmap);
            return node;
        }
        else
            return nullptr;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.empty())
            return nullptr;
        unordered_map<int,int> hashmap;
        for (int i = 0; i < inorder.size(); i++)
            hashmap[inorder[i]] = i;
        reverse(postorder.begin(), postorder.end());
        int i = 0, l = 0, r = inorder.size()-1;
        return build(postorder, i, inorder, l, r, hashmap);
    }
};

```

<br>



---------------------------
##### 297. 二叉树的序列化与反序列化
>题目描述:序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。
请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。
说明: 不要使用类的成员 / 全局 / 静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree

解题思路：将树转化为string时，前序遍历，保留树节点的值，null用 # 代表；将string创建树时，同样适用前序遍历创建

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Codec {
public:
    void DFS(TreeNode* root, stringstream& ss)
    {
        if (!root)
        {
            ss << "# ";
            return;
        }

        ss << root->val << " ";
        DFS(root->left, ss);
        DFS(root->right, ss);
    }

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root)
            return string();
        stringstream ss;
        DFS(root, ss);
        return ss.str();    
    }

    TreeNode* build(stringstream& ss)
    {
        string s;
        ss >> s;
        if (s == "#")
            return nullptr;
        else 
        {
            int val = stoi(s);
            TreeNode* node = new TreeNode(val);
            node->left = build(ss);
            node->right = build(ss);
            return node;
        }
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data.empty()) 
            return nullptr;
        stringstream ss(data);
        return build(ss);    
    }
};

```

<br>



---------------------------
##### 剑指 Offer 33. 二叉搜索树的后序遍历序列
>题目描述:输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof

解题思路：后序遍历结果为 左右根， 二叉搜索树的性质则是 左边小于根， 右边大于根，可以先分别找到 左子树 右子树 根 的分界线，然后再进行递归判断。

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
    bool ans = true;
public:
    void DFS(vector<int>& postorder, int l, int r)
    {
        if (l <= r)
        {
            int root = postorder[r];
            int mid = r-1;
            while (mid>=l && postorder[mid]>root)
                mid--;
            for (int i = mid; i >= l; i--)
                if (postorder[i] > root)
                {
                    ans = false;
                    return;
                }
            DFS(postorder, l, mid);
            DFS(postorder, mid+1, r-1);
        }
    }

    bool verifyPostorder(vector<int>& postorder) {
        if (postorder.empty())
            return true;
        DFS(postorder, 0, postorder.size()-1);
        return ans;
    }
};
```

<br>



---------------------------
##### 剑指 Offer 36. 二叉搜索树与双向链表
>题目描述:输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。
我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof

解题思路：中序遍历

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    Node* head = nullptr;
    Node* pre = nullptr;
public:
    void DFS(Node* root)
    {
        if (!root)
            return;
        DFS(root->left);
        if (!head)
        {
            head = root;
            pre = head;
        }
        else
        {
            pre->right = root;
            root->left = pre;
            pre = root;
        }
        DFS(root->right);
    }

    Node* treeToDoublyList(Node* root) {
        if (!root)
            return nullptr;
        DFS(root);
        pre->right = head;
        head->left = pre;
        return head;
    }
};

```

<br>



---------------------------
##### 98.验证二叉搜索树
>题目描述:
给定一个二叉树，判断其是否是一个有效的二叉搜索树。
假设一个二叉搜索树具有如下特征：
节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/validate-binary-search-tree

解题思路：根据BST的性质可知中序遍历的结果递增，直接中序遍历判断即可

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    TreeNode* pre = nullptr;
    bool flag = true;
public:
    void  Inorder(TreeNode* root)
    {
        if (!root)
            return;
        Inorder(root->left);
        if (!pre)
            pre = root;
        else
        {
            if (pre->val >= root->val)
            {
                flag = false;
                return;
            }
            pre = root;
        }
        Inorder(root->right);
    }

    bool isValidBST(TreeNode* root) {
        if (!root)
            return true;
        Inorder(root);
        return flag;
    }
};

```

<br>



---------------------------
##### 99.恢复二叉搜索树
>题目描述:给你二叉搜索树的根节点 root ，该树中的两个节点被错误地交换。请在不改变其结构的情况下，恢复这棵树。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/recover-binary-search-tree


解题思路：在中序遍历的过程中直接用两个指针进行指向即可。遍历过程中仍然是可能会出现一个逆序对或者两个逆序对的情况。

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
##### 96.不同的二叉搜索树
>题目描述:给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？
示例:
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-binary-search-trees

解题思路：动态规划法，dp[i] 代表长度为i的整数可以构成BST的数量，求解n时，需要从 1 ~ n 作为根，计算出左右两边可能的情况。 1 2 3 4 5

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
public:
    int numTrees(int n) {
        if (0 == n)
            return 0;
        if (1 == n)
            return 1;
        vector<int> dp(n+1, 0);
        dp[0] = 1, dp[1] = 1;
        for (int i = 2; i <= n; i++)
            for (int j = 1; j <= i; j++)
                dp[i] += dp[j-1] * dp[i-j];
        return dp[n];
    }
};

```

<br>

---------------------------
##### 95.不同的二叉搜索树2
>题目描述:给定一个整数 n，生成所有由 1 ... n 为节点所组成的 二叉搜索树 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-binary-search-trees-ii

解题思路：递归创建法，for循环遍历i， 以i为中心，再向左右递归创建左右子树，然后将得到的左右子树数组，分别拼接到i的左右。

时间复杂度：O()

空间复杂度：O()

```cpp
class Solution {
public:
    vector<TreeNode*> generateTrees(int l, int r) {
        if (l <= r)
        {
            vector<TreeNode*> ans;
            for (int k = l; k <= r; k++)
            { 
                vector<TreeNode*> left = generateTrees(l, k-1);
                vector<TreeNode*> right = generateTrees(k+1, r);
                for (int i = 0; i < left.size(); i++)
                {
                    for (int j = 0; j < right.size(); j++)
                    {
                        TreeNode* node = new TreeNode(k);
                        node->left = left[i];
                        node->right = right[j];
                        ans.push_back(node);
                    }
                }
            }
            return ans;
        }else
            return vector<TreeNode*>{nullptr};
    }

    vector<TreeNode*> generateTrees(int n) {
        if (0 == n)
            return {};
        return generateTrees(1, n);
    }
};

```

<br>


---------------------------
##### 108.将有序数组转换为二叉搜索树
>题目描述:将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。
本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree

解题思路：不断在升序数组中进行二分递归，就可以保证生成的BST高度平衡。

时间复杂度：O(N)

空间复杂度：O(logN)

```cpp
class Solution {
public:
    TreeNode* build(vector<int>& nums, int l, int r)
    {
        if (l <= r)
        {
            int mid = (l + r) >> 1;
            TreeNode* node = new TreeNode(nums[mid]);
            node->left = build(nums, l, mid-1);
            node->right = build(nums, mid+1, r);
            return node;
        }
        else
            return nullptr;
    }

    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if (nums.empty())
            return nullptr;
        int l = 0, r = nums.size()-1;
        return build(nums, l, r);
    }
};

```

<br>

---------------------------
##### 109.有序链表转换二叉搜索树
>题目描述:给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。
本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree

解题思路：该题仍然使用二分递归法创建AVL树，链表的递归使用快慢指针求解。

时间复杂度：O(N)

空间复杂度：O(logN)

```cpp
class Solution {
public:
    ListNode* findMid(ListNode* start, ListNode* end)
    {
        if(start==end || start->next==end)
            return start;
        ListNode* s = start, *f = start;
        while (1)
        {
            if (f==end || f->next==end)
                return s;
            s = s->next;
            f = f->next->next;
        }
    }

    TreeNode* build(ListNode* start, ListNode* end)
    {
        if (start==end)
            return nullptr;
        ListNode* mid = findMid(start, end);
        TreeNode* node = new TreeNode(mid->val);
        node->left = build(start, mid);
        node->right = build(mid->next, end);
        return node;
    }

    TreeNode* sortedListToBST(ListNode* head) {
        if (!head)
            return nullptr;
        return build(head, nullptr);
    }
};

```

<br>


---------------------------
##### 112.路径总和
>题目描述:给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。
说明: 叶子节点是指没有子节点的节点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/path-sum

解题思路：递归法，遍历到所有的叶子节点并进行判断。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root)
            return false;
        if (root->val==sum && !root->left && !root->right)
            return true;
        return hasPathSum(root->left, sum-root->val) || hasPathSum(root->right, sum-root->val);
    }
};

```

<br>

---------------------------
##### 113.路径总合2
>题目描述:给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。
说明: 叶子节点是指没有子节点的节点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/path-sum-ii

解题思路：DFS，用一个path 来记录走过的路径。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    vector<vector<int>> ans;
public:
    void DFS(TreeNode* root, int sum, vector<int>& path)
    {
        if (!root)
            return;
        if (root->val==sum && !root->left && !root->right)
        {
            path.push_back(root->val);
            ans.push_back(path);
            path.pop_back();
            return;
        }

        path.push_back(root->val);
        DFS(root->left, sum-root->val, path);
        DFS(root->right, sum-root->val, path);
        path.pop_back();
    }

    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if (!root)
            return {};
        vector<int> path;
        DFS(root, sum, path);
        return ans;
    }
};

```

<br>

---------------------------
##### 437.路径总合3         
>题目描述:给定一个二叉树，它的每个结点都存放着一个整数值。
找出路径和等于给定数值的路径总数。
路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/path-sum-iii

解题思路：遍历整个树的所有节点，以每一个节点为初始起点往下遍历，查看是否有路径总合相等的节点。

时间复杂度：O(N^2)

空间复杂度：O(N)

```cpp
class Solution {
    int cnt = 0;
public:
    void count(TreeNode* root, int sum)
    {
        if (!root)
            return;
        if (root->val == sum)
            cnt++;
        
        count(root->left, sum-root->val);
        count(root->right, sum-root->val);
    }

    int pathSum(TreeNode* root, int sum) {
        if (!root)
            return 0;
        count(root, sum);
        pathSum(root->left, sum);
        pathSum(root->right, sum);
        return cnt;
    }
};

```

<br>

---------------------------
##### 124.二叉树中的最大路径和
>题目描述:给定一个非空二叉树，返回其最大路径和。
本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。
注意：左子树->根->右子树 也算是路径。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-maximum-path-sum

解题思路：后序遍历的方法，不断将计算出来的结果与保留的最大值进行比较

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    int Max = INT32_MIN;
public:
    int DFS(TreeNode* root)
    {
        if (!root)
            return 0;
        int l = DFS(root->left);
        int r = DFS(root->right);
        int subTreeSum = root->val + std::max(l, r);//该节点作为子树的最大贡献值
        int sum = root->val + l + r; //该节点作为根节点的最大贡献值
        Max = std::max(Max, sum);
        return std::max(subTreeSum, 0);
    }

    int maxPathSum(TreeNode* root) {
        if (!root)
            return 0;
        DFS(root);
        return Max;
    }
};

```

<br>

---------------------------
##### 129.求根到叶子节点数字之和
>题目描述:给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。
例如，从根到叶子节点路径 1->2->3 代表数字 123。
计算从根到叶子节点生成的所有数字之和。
说明: 叶子节点是指没有子节点的节点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sum-root-to-leaf-numbers

解题思路：DFS，遍历整个二叉树并将结果存入sum总合。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    int sum = 0;
public:
    void DFS(TreeNode* root, int num)
    {
        if (!root)
            return;
        if (!root->left && !root->right)
        {
            sum += num*10 + root->val;
            return;
        }

        DFS(root->left, num*10+root->val);
        DFS(root->right, num*10+root->val);
    }

    int sumNumbers(TreeNode* root) {
        if (!root)
            return 0;
        DFS(root, 0);
        return sum;
    }
};

```

<br>

