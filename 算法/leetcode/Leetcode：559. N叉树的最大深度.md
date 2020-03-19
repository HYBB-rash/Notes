# Leetcode：[559. N叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

**Leetcode：[559. N叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)**

## Talk is cheap . Show me the code .

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
public:
    int maxDepth(Node* root) {
        queue<Node*> que;
        if(root!=NULL) que.push(root);
        Node* temp;
        int dep=0,size=0;
        while(!que.empty()){
            size=que.size();
            while(size--){
                temp=que.front();
                que.pop();
                for(int i=0;i<temp->children.size();i++){
                    que.push(temp->children[i]);
                }
            }
            dep++;
        }
        return dep;
    }
};
```

