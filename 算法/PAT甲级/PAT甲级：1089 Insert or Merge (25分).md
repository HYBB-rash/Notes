# PAT甲级：**1089** **Insert or Merge** **(25分)**

## 题干

According to Wikipedia:

**Insertion sort** iterates, consuming one input element each repetition, and growing a sorted output list. Each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. It repeats until no input elements remain.

**Merge sort** works as follows: Divide the unsorted list into N sublists, each containing 1 element (a list of 1 element is considered sorted). Then repeatedly merge two adjacent sublists to produce new sorted sublists until there is only 1 sublist remaining.

Now given the initial sequence of integers, together with a sequence which is a result of several iterations of some sorting method, can you tell which sorting method we are using?

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤100). Then in the next line, *N* integers are given as the initial sequence. The last line contains the partially sorted sequence of the *N* numbers. It is assumed that the target sequence is always ascending. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in the first line either "Insertion Sort" or "Merge Sort" to indicate the method used to obtain the partial result. Then run this method for one more iteration and output in the second line the resuling sequence. It is guaranteed that the answer is unique for each test case. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

### Sample Input 1:

```in
10
3 1 2 8 7 5 9 4 6 0
1 2 3 7 8 5 9 4 6 0

      
    
```

### Sample Output 1:

```out
Insertion Sort
1 2 3 5 7 8 9 4 6 0

      
    
```

### Sample Input 2:

```in
10
3 1 2 8 7 5 9 4 0 6
1 3 2 8 5 7 4 9 0 6

      
    
```

### Sample Output 2:

```out
Merge Sort
1 2 3 8 4 5 7 9 0 6
```

## 思路

根据两个排序的特性，我们不难发现：

1. 插入排序中，总会是前半段有序，后半段无序。
2. 归并排序中，则是一段一段的有序。

由于题目保证唯一解，我们不考虑两者交叉的情况。实际上是两者在很多情况下是无法分清的。

所以首先利用两者的特性，找到第一个次序不对的位置。再继续调查剩下的序列情况，如果与原数列都相同，说明是插入排序。否则一定是归并排序。

插入排序只需要用sort函数处理0到刚刚找到的位置即可。

而归并排序就直接对数据进行排序，等到某一步与给出的第二列数据相同，就再进行一步即可。

## code

```c++
#include <iostream>
#include <vector>
#include <algorithm>
/* run this program using the console pauser or add your own getch, system("pause") or input loop */
using namespace std;
void merge(vector<int>& sorted, vector<int>& init){
	int k = 1, flag = 1;
	while(flag){
		flag = 0;
		int i;
		if(sorted != init) flag = 1;
		k *= 2;
		for(i = 0; i + k < init.size(); i += k) sort(init.begin() + i, init.begin() + i + k);
		sort(init.begin() + i, init.end());
	}
}
int main(int argc, char** argv) {
	int n = 0, temp_split = 0;
	scanf("%d", &n);
	vector<int> init(n), sorted(n);
	for(int i = 0; i < n; i++) scanf("%d", &init[i]);
	for(int i = 0; i < n; i++) scanf("%d", &sorted[i]);
	for(int i = 1; i < n; i++){
		if(sorted[i] < sorted[i-1]){
			temp_split = i;
			break;
		} 
	}
	for(int i = temp_split; i < n; i++){
		if(init[i] != sorted[i]){
			printf("Merge Sort\n");
			merge(sorted, init);
			for(auto it = init.begin(); it != init.end(); it++){
				if(it != init.begin()) printf(" ");
				printf("%d", *it);
			}
			return 0;
		}
	}
	printf("Insertion Sort\n");
	sort(sorted.begin(), sorted.begin() + temp_split + 1);
	for(auto it = sorted.begin(); it != sorted.end(); it++){
		if(it != sorted.begin()) printf(" ");
		printf("%d", *it);
	}
	return 0;
}
```

