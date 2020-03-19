# Leetcode：[面试题 04.03. 特定深度节点链表](https://leetcode-cn.com/problems/list-of-depth-lcci/)

先贴一下自己写过一个模板，按层数遍历：

[https://www.cnblogs.com/cell-coder/p/12344619.html](https://www.cnblogs.com/cell-coder/p/12344619.html)

里面有这类题的模板

这道题就是这种类型的变种，只需要按题目意思改一下就ok

贴一下我的通过代码：

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
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<ListNode*> listOfDepth(TreeNode* tree) {
        vector<ListNode*> ans;
        queue<TreeNode*> que;
        que.push(tree);
        TreeNode* node;
        while(!que.empty()){
            int len=que.size();
            ListNode *head,*p;
            head=new ListNode(-1);
            p=head;
            while(len--){
                node=que.front();
                que.pop();
                if(node->left!=NULL) que.push(node->left);
                if(node->right!=NULL) que.push(node->right);
                p->next=new ListNode(node->val);
                p=p->next;
            }
            ans.push_back(head->next);
        }
        return ans;
    }
};
```

