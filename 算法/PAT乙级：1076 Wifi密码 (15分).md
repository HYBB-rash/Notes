# PAT乙级：**1076** **Wifi密码** **(15分)**

## 题干

下面是微博上流传的一张照片：“各位亲爱的同学们，鉴于大家有时需要使用 wifi，又怕耽误亲们的学习，现将 wifi 密码设置为下列数学题答案：A-1；B-2；C-3；D-4；请同学们自己作答，每两日一换。谢谢合作！！~”—— 老师们为了促进学生学习也是拼了…… 本题就要求你写程序把一系列题目的答案按照卷子上给出的对应关系翻译成 wifi 的密码。这里简单假设每道选择题都有 4 个选项，有且只有 1 个正确答案。

![image-20200326095227156](https://raw.githubusercontent.com/HYBB-rash/cnBlogs/master/img/20200326095229.png)

### 输入格式：

输入第一行给出一个正整数 N（≤ 100），随后 N 行，每行按照 `编号-答案` 的格式给出一道题的 4 个选项，`T` 表示正确选项，`F` 表示错误选项。选项间用空格分隔。

### 输出格式：

在一行中输出 wifi 密码。

### 输入样例：

```in
8
A-T B-F C-F D-F
C-T B-F A-F D-F
A-F D-F C-F B-T
B-T A-F C-F D-F
B-F D-T A-F C-F
A-T C-F B-F D-F
D-T B-F C-F A-F
C-T A-F B-F D-F

      
    
```

### 输出样例：

```out
13224143
```

## 思路

好像不需要思路，检测一下再统计就好了，我用的map做的。

柳婼大神的比较简单……我做复杂了。

## code

```c++
#include<iostream>
#include<string>
#include<vector>
#include<unordered_map>
using namespace std;
int main(){
	unordered_map<string,int> dic;
	dic["A-T"]=1,dic["B-T"]=2,dic["C-T"]=3,dic["D-T"]=4;
	int n=0;
	cin>>n;
	vector<int> ans(n);
	string temp;
	for(int i=0;i<n;i++){
		for(int j=0;j<4;j++){
			cin>>temp;
			if(dic[temp]) ans[i]=dic[temp];
		}
	}
	for(int i=0;i<n;i++) cout<<ans[i];
	return 0;
} 
```

柳婼大神的解法：

```c++
#include <iostream>
using namespace std;
int main() {
	string s;
	while (cin >> s)
	if(s.size() == 3 && s[2] == 'T') cout << s[0]-'A'+1;
	return 0;
}
```

这就是所谓的高下立判么，ilil。