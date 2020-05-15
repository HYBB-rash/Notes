# PAT甲级：**1136** **A Delayed Palindrome** (20分)

## 题干

Consider a positive integer *N* written in standard notation with *k*+1 digits *a**i* as *a**k*⋯*a*1*a*0 with 0≤*a**i*<10 for all *i* and *a**k*>0. Then *N* is **palindromic** if and only if *a**i*=*a**k*−*i* for all *i*. Zero is written 0 and is also palindromic by definition.

Non-palindromic numbers can be paired with palindromic ones via a series of operations. First, the non-palindromic number is reversed and the result is added to the original number. If the result is not a palindromic number, this is repeated until it gives a palindromic number. Such number is called **a delayed palindrome**. (Quoted from https://en.wikipedia.org/wiki/Palindromic_number )

Given any positive integer, you are supposed to find its paired palindromic number.

### Input Specification:

Each input file contains one test case which gives a positive integer no more than 1000 digits.

### Output Specification:

For each test case, print line by line the process of finding the palindromic number. The format of each line is the following:

```
A + B = C

      
    
```

where `A` is the original number, `B` is the reversed `A`, and `C` is their sum. `A` starts being the input number, and this process ends until `C` becomes a palindromic number -- in this case we print in the last line `C is a palindromic number.`; or if a palindromic number cannot be found in 10 iterations, print `Not found in 10 iterations.` instead.

### Sample Input 1:

```in
97152

      
    
```

### Sample Output 1:

```out
97152 + 25179 = 122331
122331 + 133221 = 255552
255552 is a palindromic number.

      
    
```

### Sample Input 2:

```in
196

      
    
```

### Sample Output 2:

```out
196 + 691 = 887
887 + 788 = 1675
1675 + 5761 = 7436
7436 + 6347 = 13783
13783 + 38731 = 52514
52514 + 41525 = 94039
94039 + 93049 = 187088
187088 + 880781 = 1067869
1067869 + 9687601 = 10755470
10755470 + 07455701 = 18211171
Not found in 10 iterations.
```

## 思路

实际上很简单，给大家提一下坑点：

1. 首次输入的数字可能已经符合条件，不需要再作运算。例如11就可以直接输出答案，不需要再运算1次.
2. 输入的数字用`long long int` 都没有办法全部存上，必须用`string` 实现模拟加法。

## code

```c++
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;
string palindromic(string num, string r_num){
	string ans;
	int flag = 0;
	for(int i = 0; i < num.size(); i++){
		int temp = num[i] + r_num[i] - 2 * '0' + flag;
		flag = temp / 10;
		ans.push_back((char) temp % 10 + '0');
	}
	if(flag) ans.push_back(flag + '0');
	reverse(ans.begin(), ans.end());
	printf("%s + %s = %s\n", num.c_str(), r_num.c_str(), ans.c_str());
	return ans;
}
int main(){
	string num;
	cin >> num;
	for(int i = 0; i < 10; i++){
		string r_temp = num;
		reverse(r_temp.begin(), r_temp.end());
		if(num == r_temp){
			printf("%s is a palindromic number.", num.c_str());
			return 0;	
		} 
		num = palindromic(num, r_temp);
	}
	printf("Not found in 10 iterations.");
	return 0;
}
```

