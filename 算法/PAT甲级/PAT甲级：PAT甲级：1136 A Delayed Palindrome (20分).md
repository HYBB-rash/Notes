# PAT甲级：PAT甲级：**1136** **A Delayed Palindrome** (20分)

## 题干

Look-and-say sequence is a sequence of integers as the following:

```
D, D1, D111, D113, D11231, D112213111, ...

      
    
```

where `D` is in [0, 9] except 1. The (n+1)st number is a kind of description of the nth number. For example, the 2nd number means that there is one `D` in the 1st number, and hence it is `D1`; the 2nd number consists of one `D` (corresponding to `D1`) and one 1 (corresponding to 11), therefore the 3rd number is `D111`; or since the 4th number is `D113`, it consists of one `D`, two 1's, and one 3, so the next number must be `D11231`. This definition works for `D` = 1 as well. Now you are supposed to calculate the Nth number in a look-and-say sequence of a given digit `D`.

### Input Specification:

Each input file contains one test case, which gives `D` (in [0, 9]) and a positive integer N (≤ 40), separated by a space.

### Output Specification:

Print in a line the Nth number in a look-and-say sequence of `D`.

### Sample Input:

```in
1 8

      
    
```

### Sample Output:

```out
1123123111
```

## 思路

一道经典的滑动窗口题目。而我遇到了一个坑点。

* `ans = ans + d[q] + to_string(p - q);`
* `ans += d[q] + to_string(p - q);`

咋一看这两句是同一个意思，但是第一句在PAT测试点3是超时的。

教训就是，别闲着没事乱写，老老实实写`+=` ，明明就更简单是不是？

具体原因不太清楚，反正遇上了，就试试换成`+=` 看对不对？

## code

```c++
#include <iostream>
#include <string>
using namespace std;
int main(){
	int n = 0;
	string d;
	d.resize(1);
	scanf("%s%d", &d[0], &n);
	for(int i = 1; i < n; i++){
		string ans;
		int p = 1, q = 0;
		d = d + "@";
		while(p < d.size()){
			if(d[p] != d[q]){
				//ans = ans + d[q] + to_string(p - q);
				ans += d[q] + to_string(p - q);
				q = p;
			}
			p++;
		}
		d = ans;
	}
	printf("%s", d.c_str());
	return 0;
}

```

