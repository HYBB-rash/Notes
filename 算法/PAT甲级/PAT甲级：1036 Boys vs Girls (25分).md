# PAT甲级：**1036** **Boys vs Girls **(25分)

## 题干

This time you are asked to tell the difference between the lowest grade of all the male students and the highest grade of all the female students.

### Input Specification:

Each input file contains one test case. Each case contains a positive integer *N*, followed by *N* lines of student information. Each line contains a student's `name`, `gender`, `ID` and `grade`, separated by a space, where `name` and `ID` are strings of no more than 10 characters with no space, `gender` is either `F` (female) or `M` (male), and `grade` is an integer between 0 and 100. It is guaranteed that all the grades are distinct.

### Output Specification:

For each test case, output in 3 lines. The first line gives the name and ID of the female student with the highest grade, and the second line gives that of the male student with the lowest grade. The third line gives the difference *g**r**a**d**e**F*−*g**r**a**d**e**M*. If one such kind of student is missing, output `Absent` in the corresponding line, and output `NA` in the third line instead.

### Sample Input 1:

```in
3
Joe M Math990112 89
Mike M CS991301 100
Mary F EE990830 95

      
    
```

### Sample Output 1:

```out
Mary EE990830
Joe Math990112
6

      
    
```

### Sample Input 2:

```in
1
Jean M AA980920 60

      
    
```

### Sample Output 2:

```out
Absent
Jean AA980920
NA
```

## 思路

上sort函数，直接写一个符合题意的`cmp`函数，使得数组开头第一个就是女生最高分，最后一个是男生最低分。

然后就很简单了~

至于`cmp`函数怎么写，先比较性别，男生往后排，女生往前排。性别相同时按成绩从高到低排即可~

第二种方法也挺简单，就搜索一遍数组找到男女生的最大值和最小值也可以，复杂度应该比直接排序要小~

这种就不说了，比较简单，看看这种新奇点的方法能不能给自己带来点启发吧~

## code

```c++
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
struct stu{
	string name;
	char sex;
	string ID;
	int grade;
};
bool cmp(stu a, stu b){//只需写一个cmp函数即可~
	if(a.sex == b.sex) return a.grade > b.grade;
	return a.sex < b.sex;
}
void P(stu s, char sex){
	if(s.sex == sex) printf("%s %s\n", s.name.c_str(), s.ID.c_str());
	else printf("Absent\n");
}
int main(int argc, char** argv) {
	int n = 0, flag = 0;
	scanf("%d", &n);
	vector<stu> score(n);
	for(int i = 0; i < n; i++) score[i].name.resize(15), score[i].ID.resize(15);
	for(int i = 0; i < n; i++) scanf("%s %c %s %d", &score[i].name[0], &score[i].sex, &score[i].ID[0], &score[i].grade);
	sort(score.begin(), score.end(), cmp);
	if(!(score[0].sex == 'F' && score[score.size() - 1].sex == 'M')) flag = 1;
	P(score[0], 'F');
	P(score[score.size() - 1], 'M');
	if(flag) printf("NA\n");
	else printf("%d\n", score[0].grade - score[score.size() - 1].grade);
	return 0;
}
```

## 结果

| 提交时间            | 状态     | 分数 | 题目                                                         | 编译器    | 耗时 | 用户  |
| ------------------- | -------- | ---- | ------------------------------------------------------------ | --------- | ---- | ----- |
| 2020/05/05 09:33:39 | 答案正确 | 25   | [1036](https://pintia.cn/problem-sets/994805342720868352/problems/994805453203030016) | C++ (g++) | 4 ms | rash! |

| 测试点 | 结果     | 耗时 | 内存   |
| ------ | -------- | ---- | ------ |
| 0      | 答案正确 | 3 ms | 384 KB |
| 1      | 答案正确 | 4 ms | 384 KB |
| 2      | 答案正确 | 3 ms | 384 KB |
| 3      | 答案正确 | 3 ms | 384 KB |