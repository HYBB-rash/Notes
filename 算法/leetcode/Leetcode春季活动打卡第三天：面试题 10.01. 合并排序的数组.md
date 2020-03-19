# Leetcode春季活动打卡第三天：[面试题 10.01. 合并排序的数组](https://leetcode-cn.com/problems/sorted-merge-lcci/)

**Leetcode春季活动打卡第三天：[面试题 10.01. 合并排序的数组](https://leetcode-cn.com/problems/sorted-merge-lcci/)**

## 思路

这道题，两个数组原本就有序。于是我们采用双指针法完成题目。

又由于A本身就预留了足够的空间，于是我们的双指针就逆向执行，即从大到小移动，直接从A数组的最后开始覆盖。这样就不需要引用额外的临时数组。

不用考虑A数组的有效值是否会被覆盖。因为只有当A数组的位置没给够的情况下才会出现覆盖有效值的情况。

## Talk is cheap . Show me the code .

```c++
class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        int p=m-1,q=n-1,cnt=A.size()-1;
        while(p>=0&&q>=0){
            if(A[p]<=B[q]) A[cnt--]=B[q--];
            else if(A[p]>B[q]) A[cnt--]=A[p--];
        }
        while(q>=0) A[cnt--]=B[q--];
        while(p>=0) A[cnt--]=A[p--];
    }
};
```

