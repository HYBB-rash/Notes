# Leetcode:[637. 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)

**Leetcode:[637. 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)**

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
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> ans;
        queue<TreeNode*> que;
        TreeNode* node;
        int size=0;
        double temp=0;
        if(root!=NULL) que.push(root);
        while(!que.empty()){
            size=que.size();
            for(int i=0;i<size;i++){
                node=que.front();
                que.pop();
                temp+=node->val;
                if(node->left!=NULL) que.push(node->left);
                if(node->right!=NULL) que.push(node->right);
            }
            ans.push_back(temp/size);
            temp=0;
        }
        return ans;
    }
};
```

