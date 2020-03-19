# Leetcode：[169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

[传送门](https://leetcode-cn.com/problems/majority-element/)

## 思路

1. 一开始想到的一个很简单的做法就是hash法，直接利用打表记录次数再输出结果。
2. 而利用BM算法可以令算法复杂度同样也在$O（n）$的情况下，将空间复杂度也下降到1(好像也叫投票法)
3. 不谈证明，谈谈理解：
   * 如果一个数是众数，那它一定能将其他数字全部抵消变成正数，即使在局部中不是众数
   * 因此不论怎么样，他一定能慢慢的将所有其他数字全部抵消，重新更迭成为候选众数

## 代码实现

```c++
    int majorityElement(vector<int>& nums) {
        int candiate=0,cnt=0;
        for(int num:nums){
            if(cnt==0) candiate=num;
            if(candiate==num) cnt++;
            else cnt--;
        }
        return candiate;
    }
```

