# **PAT甲级： 1027** **Colors in Mars** **(20point(s))**

## 题干

People in Mars represent the colors in their computers in a similar way as the Earth people. That is, a color is represented by a 6-digit number, where the first 2 digits are for `Red`, the middle 2 digits for `Green`, and the last 2 digits for `Blue`. The only difference is that they use radix 13 (0-9 and A-C) instead of 16. Now given a color in three decimal numbers (each between 0 and 168), you are supposed to output their Mars RGB values.

### Input Specification:

Each input file contains one test case which occupies a line containing the three decimal color values.

### Output Specification:

For each test case you should output the Mars RGB value in the following format: first output `#`, then followed by a 6-digit number where all the English characters must be upper-cased. If a single color is only 1-digit long, you must print a `0` to its left.

### Sample Input:

```in
15 43 71

      
    
```

### Sample Output:

```out
#123456
```

## 思路

用一个简单粗暴的方法：把0-12全映射出来，然后求余就行。

## code

```c++
#include <iostream>
#include <unordered_map>
#include <string>
#include <algorithm>
/* run this program using the console pauser or add your own getch, system("pause") or input loop */
using namespace std;
unordered_map<int, string> Mars;
void init(){
	Mars[1] = "1", Mars[2] = "2", Mars[3] = "3", Mars[4] = "4", Mars[5] = "5", Mars[6] = "6";
	Mars[7] = "7", Mars[8] = "8", Mars[9] = "9", Mars[10] = "A", Mars[11] = "B", Mars[12] = "C";
	Mars[0] = "0";
}
int main(int argc, char** argv) {
	init();
	string ans = "#";
	for(int i = 0; i < 3; i++){
		int temp;
		scanf("%d", &temp);
		string t = "";
		for(int i = 0; i < 2; i++){
			t.append(Mars[temp % 13]);
			temp /= 13;
		}
		reverse(t.begin(), t.end());
		ans.append(t);
	}
	cout<<ans;
	return 0;
}
```

