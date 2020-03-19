# Leetcode：[110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

**Leetcode：[110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)**

点链接就能看到原题啦~

**关于AVL的判断函数写法，请跳转：**[平衡二叉树的判断](https://www.cnblogs.com/cell-coder/p/12355506.html)

废话不说直接上代码吧~主要的解析的都在上面的链接里了

## 自顶向下写法

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
    int getHeight(TreeNode* root){
        if(root==NULL) return 0;
        return max(getHeight(root->right),getHeight(root->left))+1;
    }
    bool isBalanced(TreeNode* root) {
        if(root==NULL) return true;
        if(isBalanced(root->left)&&isBalanced(root->right))
            if(abs(getHeight(root->left)-getHeight(root->right))<2)
                return true;
        return false;
    }
};
```

## 自底向上写法

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
    int isBalancedHelper(TreeNode* root,int& height){
        if(root==NULL){
            height=0;
            return true;
        }
        int left,right;
        if(isBalancedHelper(root->right,right)&&isBalancedHelper(root->left,left)&&abs(left-right)<2){
            height=max(left,right)+1;
            return true;
        }
        return false;
    }
    bool isBalanced(TreeNode* root) {
        int height=0;
        return isBalancedHelper(root,height);
    }
};
```

