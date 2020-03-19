# Leetcode:[543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

**Leetcode:[543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)**

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
    int cnt=-1;
    int getHeight(TreeNode* root,int& height){//自顶向上写法
        if(root==NULL){
            height=0;
            return 0;
        } 
        int left,right;
        getHeight(root->left,left),getHeight(root->right,right);
        height=max(left,right)+1;
        if(left+right>cnt) cnt=left+right;
        return cnt;
    }
    int diameterOfBinaryTree(TreeNode* root) {
        int height=0;
        return getHeight(root,height);
    }
};
```

