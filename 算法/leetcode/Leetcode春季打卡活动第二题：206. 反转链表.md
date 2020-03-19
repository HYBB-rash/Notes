# Leetcode春季打卡活动 第二题：[206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

#### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

## Talk is cheap . Show me the code .

```c++
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
    ListNode* h;
    void DFS(ListNode* head){
        if(head==NULL) return ;
        if(head->next==NULL){
            h=head;
            return;
        }
        DFS(head->next);
        head->next->next=head;
        head->next=NULL;
    }
    ListNode* reverseList(ListNode* head) {
        DFS(head);
        return h;
    }
};
```

