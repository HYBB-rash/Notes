# PAT甲级：**1064** **Complete Binary Search Tree** **(30分)**

## 题干

A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
- Both the left and right subtrees must also be binary search trees.

A Complete Binary Tree (CBT) is a tree that is completely filled, with the possible exception of the bottom level, which is filled from left to right.

Now given a sequence of distinct non-negative integer keys, a unique BST can be constructed if it is required that the tree must also be a CBT. You are supposed to output the level order traversal sequence of this BST.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤1000). Then *N* distinct non-negative integer keys are given in the next line. All the numbers in a line are separated by a space and are no greater than 2000.

### Output Specification:

For each test case, print in one line the level order traversal sequence of the corresponding complete binary search tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

### Sample Input:

```in
10
1 2 3 4 5 6 7 8 9 0

      
    
```

### Sample Output:

```out
6 3 8 1 5 7 9 0 2 4
```

## 思路

抓住两个点：

* BST的中序历遍的结果是有序的。
* 用数组建CBT可以完全利用空间，不必考虑具体的长度问题，

我们利用这两个点，将原数据进行排序后，递归建立一个数组树。建立好之后，数组的结果就是层序历遍的结果。

## code

```c++
#include <iostream>
#include <algorithm>
#include <vector>
/* run this program using the console pauser or add your own getch, system("pause") or input loop */
using namespace std;
int cnt = 0;
void dfs(vector<int> &order, vector<int> &tree, int index){
	if(index >= order.size()) return;
	dfs(order, tree, index * 2 + 1);
	tree[index] = order[cnt++];
	dfs(order, tree, index * 2 + 2); 
}
int main(int argc, char** argv) {
	int n = 0;
	scanf("%d", &n);
	vector<int> order(n), tree(n);
	for(int i = 0; i < n; i++) scanf("%d", &order[i]);
	sort(order.begin(), order.end());
	dfs(order, tree, 0);
	for(auto it = tree.begin(); it != tree.end(); it++){
		if(it != tree.begin()) printf(" ");
		printf("%d", *it);
	}
	return 0;
}
```

