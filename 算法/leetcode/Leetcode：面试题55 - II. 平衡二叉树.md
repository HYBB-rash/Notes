# Leetcode：[面试题55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

**Leetcode：[面试题55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)**

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
    bool isBalance(TreeNode* root,int& height){
        if(root==NULL){
            height=0;
            return true;
        }
        int left,right;
        if(isBalance(root->left,left)&&isBalance(root->right,right)){
            if(abs(left-right)<2){
                height=max(left,right)+1;
                return true;
            } 
        }
        return false;
    }
    bool isBalanced(TreeNode* root) {
        int height=0;
        return isBalance(root,height);
    }
};
```

