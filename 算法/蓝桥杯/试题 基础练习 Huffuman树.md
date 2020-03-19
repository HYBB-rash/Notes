## 试题 基础练习 Huffuman树

**[试题 基础练习 Huffuman树](http://lx.lanqiao.cn/problem.page?gpid=T69)**

​		翻了翻网上，基本都是暴力排序。我就提供一个**最小堆**的写法吧！

​		[点击这里，跳转查看最小堆插入删除函数的简单写法](https://www.cnblogs.com/cell-coder/p/12370317.html)

## Talk is cheap . Show me the code.

```c++
#include<iostream>
#include<algorithm>
#include<limits.h>
#include<vector>
using namespace std;
vector<int> minHeap;
void insert(int x) {
	minHeap.push_back(x);
	int child=minHeap.size()-1;
	while(minHeap[child/2]>minHeap[child]){
		swap(minHeap[child/2],minHeap[child]);
		child/=2;
	}
}
int Delete(){
	int value=minHeap[1];
	swap(minHeap[1],minHeap[minHeap.size()-1]);
	minHeap.pop_back();
	int parent=1,child=0;
	for(parent=1;parent*2<minHeap.size();parent=child){
		child=parent*2;
		if(child!=minHeap.size()-1&&minHeap[child+1]<minHeap[child]) child++;
		if(minHeap[parent]<minHeap[child]) break;
		else swap(minHeap[parent],minHeap[child]);
	}
	return value;
}
int main(){
	int n,temp=0;
	minHeap.push_back(INT_MIN);
	cin>>n;
	for(int i=0;i<n;i++){
		cin>>temp;
		insert(temp);
	}
	n=0;
	while(minHeap.size()>2){
		temp=Delete()+Delete();
		n+=temp;
		insert(temp);
	}
	cout<<n<<endl;
	return 0;
}
```

