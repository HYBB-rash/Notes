# Leetcode：[230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

**Leetcode：[230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)**

## 思路：

利用BST的中序历遍的结果为其排序后的结果，我们可以利用其特性直接找到第k个中序遍历元素，即为问题答案。

# Talk is cheap . Show me the code .

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
    int cnt=0,ans=0;
    int kthSmallest(TreeNode* root, int k) {
        if(root==NULL) return ans;
        if(cnt==k) return ans;
        ans=kthSmallest(root->left,k);
        cnt++;
        if(cnt==k) ans=root->val;
        ans=kthSmallest(root->right,k);
        return ans;
    }
};
```

