# Leetcode：[1305. 两棵二叉搜索树中的所有元素](https://leetcode-cn.com/problems/all-elements-in-two-binary-search-trees/)

**Leetcode：[1305. 两棵二叉搜索树中的所有元素](https://leetcode-cn.com/problems/all-elements-in-two-binary-search-trees/)**

## 思路

1. BST树中序历遍**有序**。
2. 利用双指针法可以在$O(n)$的复杂度内完成排序。

基于上述两个点，可以很简单的做出这道题。

1. 先中序历遍得到两个有序的数列。
2. 利用双指针法，只需历遍一次就可以得到升序的答案。

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
    void inOrder(TreeNode *root,vector<int>& order){
        if(root==NULL) return ;
        inOrder(root->left,order);
        order.push_back(root->val);
        inOrder(root->right,order);
    }
    vector<int> getAllElements(TreeNode* root1, TreeNode* root2) {
        vector<int> a,b,ans;
        inOrder(root1,a),inOrder(root2,b);
        ans.resize(a.size()+b.size());
        int p=0,q=0,cnt=0;
        while(p<a.size()&&q<b.size()){
            if(a[p]<=b[q]) ans[cnt++]=a[p++];
            else if(a[p]>b[q]) ans[cnt++]=b[q++];
        }
        while(p<a.size()) ans[cnt++]=a[p++];
        while(q<b.size()) ans[cnt++]=b[q++];
        return ans;
    }
};
```

