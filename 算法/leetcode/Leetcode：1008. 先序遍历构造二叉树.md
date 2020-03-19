# Leetcode：[1008. 先序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/)

**Leetcode：[1008. 先序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/)**

## 思路

既然给了一个遍历结果让我们建树，那就是要需要前序中序建树咯~

题目给的树是一颗BST树，说明中序历遍是有序的。最简单的想法自然是先排序再建树。

但是排序似乎是不需要的，因为BST左子树必小于右子树，于是我们只需要确定哪个点比根节点要大，就能说明右子树是从哪里开始的。

先序遍历无法确定的东西有一个被确定了，左子树的范围就很简单了啊。

于是不需要先排序，只要历遍一下就可以了。降低了一点复杂度。

## Talk is cheap . Show me the code .

```c++
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
    TreeNode* built(int root,int start,int end,vector<int> preOrder){
        if(start>end) return NULL;
        TreeNode* treenode=new TreeNode(preOrder[root]);
        int index=start;
        while(index<=end&&preOrder[root]>=preOrder[index]) index++;
        treenode->left=built(root+1,start+1,index-1,preOrder);
        treenode->right=built(root+index-start,index,end,preOrder);
        return treenode;
    }
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        TreeNode *node=built(0,0,preorder.size()-1,preorder);
        return node;
    }
};
```

