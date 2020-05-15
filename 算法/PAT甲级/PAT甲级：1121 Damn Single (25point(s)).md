# PAT甲级：**1121** **Damn Single** **(25point(s))**

## 题干

"Damn Single (单身狗)" is the Chinese nickname for someone who is being single. You are supposed to find those who are alone in a big party, so they can be taken care of.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤ 50,000), the total number of couples. Then N lines of the couples follow, each gives a couple of ID's which are 5-digit numbers (i.e. from 00000 to 99999). After the list of couples, there is a positive integer M (≤ 10,000) followed by M ID's of the party guests. The numbers are separated by spaces. It is guaranteed that nobody is having bigamous marriage (重婚) or dangling with more than one companion.

### Output Specification:

First print in a line the total number of lonely guests. Then in the next line, print their ID's in increasing order. The numbers must be separated by exactly 1 space, and there must be no extra space at the end of the line.

### Sample Input:

```in
3
11111 22222
33333 44444
55555 66666
7
55555 44444 10000 88888 22222 11111 23333

      
    
```

### Sample Output:

```out
5
10000 23333 44444 55555 88888

      
    
```

## code

```c++
#include <iostream>
#include <unordered_map>
#include <vector>
#include <set>
#include <algorithm> 
/* run this program using the console pauser or add your own getch, system("pause") or input loop */
using namespace std;
int main(int argc, char** argv) {
	unordered_map<int, int> couple, isE;
	int n = 0;
	scanf("%d", &n);
	int c = 0, p = 0;
	for(int i = 0; i < n; i++){
		scanf("%d%d", &c, &p);
		couple[c] = p, couple[p] = c;
	}
	scanf("%d", &n);
	vector<int> party(n);
	set<int> ans,a;
	int temp = 0;
	for(int i = 0; i < n; i++) scanf("%d", &party[i]); 
	set<int> b(party.begin(), party.end());
	for(int i = 0; i < n; i++){
		isE[party[i]]++;
		if(couple.find(party[i]) != couple.end()) isE[couple[party[i]]]++;
	}
	for(pair<int, int> record:isE){
		if(record.second == 2) ans.insert(record.first);
	}
	int cnt = 0;
	set_difference(b.begin(), b.end(), ans.begin(), ans.end(), inserter(a, a.begin()));
	printf("%d\n", a.size());
	for(int v:a){
		if(cnt != 0) printf(" ");
		printf("%05d", v);
		cnt++;
	}
	return 0;
}
```

## 思路

找出是情侣的集合，再对原集合做差集即可。