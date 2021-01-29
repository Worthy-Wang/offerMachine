- [十三.图专题](#十三图专题)
    - [133.克隆图](#133克隆图)


### 十三.图专题

---------------------------
##### 133.克隆图
>题目描述:给你无向 连通 图中一个节点的引用，请你返回该图的 深拷贝（克隆）。
图中的每个节点都包含它的值 val（int） 和其邻居的列表（list[Node]）。
class Node {
    public int val;
    public List<Node> neighbors;
}
测试用例格式：
简单起见，每个节点的值都和它的索引相同。例如，第一个节点值为 1（val = 1），第二个节点值为 2（val = 2），以此类推。该图在测试用例中使用邻接列表表示。
邻接列表 是用于表示有限图的无序列表的集合。每个列表都描述了图中节点的邻居集。
给定节点将始终是图中的第一个节点（值为 1）。你必须将 给定节点的拷贝 作为对克隆图的引用返回。
节点数不超过 100 。
每个节点值 Node.val 都是唯一的，1 <= Node.val <= 100。
无向图是一个简单图，这意味着图中没有重复的边，也没有自环。
由于图是无向的，如果节点 p 是节点 q 的邻居，那么节点 q 也必须是节点 p 的邻居。
图是连通图，你可以从给定节点访问到所有节点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/clone-graph

解题思路：设置哈希表<旧节点，新节点>，若该节点已经创建，那么不需要再创建邻接表，直接返回；如果没有创建该节点的话那么就要递归创建新节点。

时间复杂度：O(N)

空间复杂度：O(N)

```cpp
class Solution {
    unordered_map<Node*,Node*> hashmap;//<旧，新>
public:
    Node* cloneGraph(Node* node) {
        if (!node)
            return nullptr;
        if (hashmap.count(node))
            return hashmap[node];
        Node* newNode = new Node(node->val);
        hashmap[node] = newNode;
        for (auto& e: node->neighbors)
            newNode->neighbors.push_back(cloneGraph(e));
        return newNode;
    }
};

```
