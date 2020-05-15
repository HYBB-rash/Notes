# PAT甲级：1066 Root of AVL Tree (25分)

## 题干

An AVL tree is a self-balancing binary search tree. In an AVL tree, the heights of the two child subtrees of any node differ by at most one; if at any time they differ by more than one, rebalancing is done to restore this property. Figures 1-4 illustrate the rotation rules.



![img](https://gitee.com/hybb0430/picture_bed/raw/master/img/20200507075611.jpg) ![img](https://gitee.com/hybb0430/picture_bed/raw/master/img/20200507075739.jpg)



![img](https://gitee.com/hybb0430/picture_bed/raw/master/img/20200507075736.jpg) ![img](https://gitee.com/hybb0430/picture_bed/raw/master/img/20200507075731.jpg)

Now given a sequence of insertions, you are supposed to tell the root of the resulting AVL tree.



### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤20) which is the total number of keys to be inserted. Then *N* distinct integer keys are given in the next line. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the root of the resulting AVL tree in one line.

### Sample Input 1:

```in
5
88 70 61 96 120

      
    
```

### Sample Output 1:

```out
70

      
    
```

### Sample Input 2:

```
7
88 70 61 96 120 90 65

      
    
```

### Sample Output 2:

```
88
```

## 思路

模拟一下AVL树的过程。

四个旋转方法，左单旋，右单旋，左右双旋，右左双旋。具体看代码，函数名写得挺清楚的。

还需要一个高度函数，递归一下就得出来了。

最后再弄一个`insert` 函数，注意AVL树的特点，左右子树的高度差为2时必须发生平衡旋转。

## code

```c++
#include <iostream>
using namespace std;
struct node{
	int data;
	node *left, *right;
	node(int data){this->data = data, this->left = this->right = NULL;}
};
node* rotate_left(node* root){
	node* temp = root->left;
	root->left = temp->right;
	temp->right = root;
	return temp;
}
node* rotate_right(node* root){
	node* temp = root->right;
	root->right = temp->left;
	temp->left = root;
	return temp;
}
node* rotate_left_right(node* root){
	root->right = rotate_left(root->right);
	return rotate_right(root);
}
node* rotate_right_left(node* root){
	root->left = rotate_right(root->left);
	return rotate_left(root);
}
int getHeight(node* root){
	if(root == NULL) return 0;
	return max(getHeight(root->right), getHeight(root->left)) + 1;
}
node* insert(int data, node* root){
	if(root == NULL) root = new node(data);
	else if(data > root->data) {
		root->right = insert(data, root->right);
		if(getHeight(root->right) - getHeight(root->left) >= 2)
			root = root->right->data > data ? rotate_left_right(root) : rotate_right(root);
	}
	else {
		root->left = insert(data, root->left);
		if(getHeight(root->left) - getHeight(root->right) >= 2)
			root = root->left->data < data ? rotate_right_left(root) : rotate_left(root);
	}
	return root;
}
int main(){
	int n = 0, temp = 0;
	scanf("%d", &n);
	node *root = NULL;
	for(int i = 0; i < n; i++){
		scanf("%d", &temp);
		root = insert(temp, root);
	}
	printf("%d", root->data);
	return 0;
}

```

