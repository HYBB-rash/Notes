# 蓝桥杯：ALGO-236 大小写转换

[传送门](http://lx.lanqiao.cn/problem.page?gpid=T596)

比较简单，直接转换。

**题解**

```c++
#include<stdio.h>
#include<iostream>
#include<string>
using namespace std;
int main() {
	string a;
	cin>>a;
	for(int i=0;i<a.length();i++){
		if(a[i]>='a'&&a[i]<='z') a[i]=a[i]-'a'+'A';
		else if(a[i]>='A'&&a[i]<='Z') a[i]=a[i]-'A'+'a';
	}
	cout<<a<<endl;
	return 0;
}
```

