# Leetcode：[105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)&[106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

**Leetcode：[105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)&[106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)**

这道题是经典的模板题啦~

**用前序中序后序遍历结果建树的模板请跳转到：[前中后序建立树或者直接历遍](https://www.cnblogs.com/cell-coder/p/12348451.html)**

直接默写！！！

## 105. 从前序与中序遍历序列构造二叉树

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
    vector<int> preOrder,inOrder;
    TreeNode* built(int root,int start,int end,TreeNode* tree){
        if(start>end) return NULL;
        if(tree==NULL) tree=new TreeNode(preOrder[root]);
        int index=start;
        while(inOrder[index]!=preOrder[root]) index++;
        tree->left=built(root+1,start,index-1,tree->left);
        tree->right=built(root+index-start+1,index+1,end,tree->right);
        return tree;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        preOrder=preorder,inOrder=inorder;
        TreeNode* tree=NULL;
        tree=built(0,0,preOrder.size()-1,tree);
        return tree;
    }
};
```



## 106. 从中序与后序遍历序列构造二叉树

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
    vector<int> inOrder,postOrder;
    TreeNode* build(int root,int start,int end,TreeNode* tree){
        if(start>end) return NULL;
        if(tree==NULL) tree=new TreeNode(postOrder[root]);
        int index=start;
        while(postOrder[root]!=inOrder[index]) index++;
        tree->left=build(root-end+index-1,start,index-1,tree->left);
        tree->right=build(root-1,index+1,end,tree->right);
        return tree;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        inOrder=inorder,postOrder=postorder;
        TreeNode* tree=NULL;
        tree=build(inOrder.size()-1,0,inOrder.size()-1,tree);
        return tree;
    }
};
```

