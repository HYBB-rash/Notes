# Leetcode 春季打卡活动 第一题:[225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

**Leetcode 春季打卡活动 第一题:[225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)**

## 解题思路

这里用了非常简单的思路，就是在push函数上做点操作，让队头总是最后一个元素即可。
也就是说，每新进一个新元素，就把前面的所有元素逐个弹出放到队尾即可。

## Talk is cheap . Show me the code .

```c++
class MyStack {
public:
    queue<int> que;
    int size,temp;
    /** Initialize your data structure here. */
    MyStack() {
        size=0;
    }
    
    /** Push element x onto stack. */
    void push(int x) {
        size=que.size();
        que.push(x);
        while(size--){
            temp=que.front();
            que.pop();
            que.push(temp);
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        if(!empty()){
            temp=que.front();
            que.pop();
            return temp;
        }
        return 0;
    }
    
    /** Get the top element. */
    int top() {
        if(!empty()) return que.front();
        return 0;
    }
    
    /** Returns whether the stack is empty. */
    bool empty() {
        return que.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */

作者：keep-smile
链接：https://leetcode-cn.com/problems/implement-stack-using-queues/solution/bi-jiao-jian-dan-de-cao-zuo-fang-shi-rang-dui-tou-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

