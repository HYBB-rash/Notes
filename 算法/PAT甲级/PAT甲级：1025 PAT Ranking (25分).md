# PAT甲级：**1025** **PAT Ranking** (25分)

## 题干

Programming Ability Test (PAT) is organized by the College of Computer Science and Technology of Zhejiang University. Each test is supposed to run simultaneously in several places, and the ranklists will be merged immediately after the test. Now it is your job to write a program to correctly merge all the ranklists and generate the final rank.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive number *N* (≤100), the number of test locations. Then *N* ranklists follow, each starts with a line containing a positive integer *K* (≤300), the number of testees, and then *K* lines containing the registration number (a 13-digit number) and the total score of each testee. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, first print in one line the total number of testees. Then print the final ranklist in the following format:

```
registration_number final_rank location_number local_rank

      
    
```

The locations are numbered from 1 to *N*. The output must be sorted in nondecreasing order of the final ranks. The testees with the same score must have the same rank, and the output must be sorted in nondecreasing order of their registration numbers.

### Sample Input:

```in
2
5
1234567890001 95
1234567890005 100
1234567890003 95
1234567890002 77
1234567890004 85
4
1234567890013 65
1234567890011 25
1234567890014 100
1234567890012 85

      
    
```

### Sample Output:

```out
9
1234567890005 1 1 1
1234567890014 1 2 1
1234567890001 3 1 2
1234567890003 3 1 2
1234567890004 5 1 4
1234567890012 5 2 2
1234567890002 7 1 5
1234567890013 8 2 3
1234567890011 9 2 4

      
    
```

## 思路

滑动窗口经典用法。先每个组排序，设定好排名，再混在一起，再次设定排名即可。

加一个哨兵，可以方便处理。

注意当成绩相同时，按ID大小排序。

用`long long int` 记得按13位格式补零，害怕自己补不好就用`string` 。

## code

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
struct pat_stu{
	long long int ID;
	int loca, grade, rank, gobal_rank;
};
bool cmp(pat_stu a, pat_stu b){
	if(a.grade == b.grade) return a.ID < b.ID;
	return a.grade > b.grade;
}
int main(){
	int n = 0, num = 0;
	vector<pat_stu> list, temp;
	pat_stu shao;
	shao.grade = -10;
	scanf("%d", &n);
	for(int i = 0; i < n; i++){
		scanf("%d", &num);
		temp.resize(num);
		for(int j = 0; j < num; j++){
			temp[j].loca = i + 1;
			scanf("%lld %d", &temp[j].ID, &temp[j].grade);
		}
		sort(temp.begin(), temp.end(), cmp);
		temp.push_back(shao);
		int p = 0, q = 0;
		while(p < temp.size()){
			if(temp[q].grade != temp[p].grade) q = p;
			temp[p].rank = q + 1;
			list.push_back(temp[p]);
			p++;
		}
		list.pop_back();
		temp.clear();
	}
	sort(list.begin(), list.end(), cmp);
	printf("%d\n", list.size());
	list.push_back(shao);
	int p = 0, q = 0;
	while(p < list.size()){
		if(list[q].grade != list[p].grade) q = p;
		list[p].gobal_rank = q + 1;
		p++;
	}
	list.pop_back();
	for(pat_stu s:list) printf("%013lld %d %d %d\n", s.ID, s.gobal_rank, s.loca, s.rank);
	return 0;
} 

```



