# Leetcode：[530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

**Leetcode：[530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)**

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
    int min=INT_MAX;
    TreeNode* preNode=NULL;
    void DFS(TreeNode* root){
        if(root==NULL) return;
        DFS(root->left);
        if(preNode!=NULL&&abs(root->val-preNode->val)<min) min=abs(root->val-preNode->val);
        preNode=root;
        DFS(root->right);
    }
    int getMinimumDifference(TreeNode* root) {
        DFS(root);
        return min;
    }
};
```

