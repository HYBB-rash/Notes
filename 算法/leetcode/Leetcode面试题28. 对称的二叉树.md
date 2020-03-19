# Leetcode:[面试题28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

**Leetcode:[面试题28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)**

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
    bool isS(TreeNode* left,TreeNode* right){
        if(left==NULL&&right==NULL) return true;
        if(left==NULL||right==NULL) return false;
        if(left->val==right->val) return isS(left->left,right->right)&&isS(left->right,right->left);
        return false;
    }
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        return isS(root->left,root->right);
    }
};
```

