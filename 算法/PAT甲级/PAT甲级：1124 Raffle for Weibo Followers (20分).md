# PAT甲级：**1124** **Raffle for Weibo Followers** (20分)

## 题干

John got a full mark on PAT. He was so happy that he decided to hold a raffle（抽奖） for his followers on Weibo -- that is, he would select winners from every N followers who forwarded his post, and give away gifts. Now you are supposed to help him generate the list of winners.

### Input Specification:

Each input file contains one test case. For each case, the first line gives three positive integers M (≤ 1000), N and S, being the total number of forwards, the skip number of winners, and the index of the first winner (the indices start from 1). Then M lines follow, each gives the nickname (a nonempty string of no more than 20 characters, with no white space or return) of a follower who has forwarded John's post.

Note: it is possible that someone would forward more than once, but no one can win more than once. Hence if the current candidate of a winner has won before, we must skip him/her and consider the next one.

### Output Specification:

For each case, print the list of winners in the same order as in the input, each nickname occupies a line. If there is no winner yet, print `Keep going...` instead.

### Sample Input 1:

```in
9 3 2
Imgonnawin!
PickMe
PickMeMeMeee
LookHere
Imgonnawin!
TryAgainAgain
TryAgainAgain
Imgonnawin!
TryAgainAgain

      
    
```

### Sample Output 1:

```out
PickMe
Imgonnawin!
TryAgainAgain

      
    
```

### Sample Input 2:

```in
2 3 5
Imgonnawin!
PickMe

      
    
```

### Sample Output 2:

```out
Keep going...
```

## 思路

1. 需要输出`keep going…` 的时候，一定是起始点没有在范围里，只需判断一下起始点的位置特判输出它。
2. 接下来就直接按照题意进行编写。比较简单。

## code

```c++
#include <iostream>
#include <string>
#include <vector>
#include <unordered_map>
using namespace std;
int main(){
	int n_user = 0, step = 0, index = 0;
	scanf("%d%d%d", &n_user, &step, &index);
	vector<string> list(n_user);
	unordered_map<string, bool> dic;
	for(int i = 0; i < n_user; i++){
		list[i].resize(25);
		scanf("%s", &list[i][0]);
	}
	if(index - 1 >= list.size()){
		printf("Keep going...");
		return 0;
	}
	for(int i = index - 1; i < list.size(); ){
		if(!dic[list[i]]){
			dic[list[i]] = true;
			printf("%s\n", list[i].c_str());
			i += step;
		}else i++;
	}
	return 0;
}
```

