# Leetcode春季打卡第四天：[994. 腐烂的橘子](https://leetcode-cn.com/problems/rotting-oranges/)

**Leetcode春季打卡第四天：[994. 腐烂的橘子](https://leetcode-cn.com/problems/rotting-oranges/)**

## 思路

1. 思路是采用广度优先搜索，一层一层遍历。
2. 首先先扫描矩阵，将坏橘子放进队列，记录正常橘子的个数。
3. 正常橘子个数为零，直接返回0
4. 不为零就开始BFS
5. 开始按层数层序遍历，将队列里的坏橘子放出来，去找会被污染的正常橘子，将其加入队列。
6. 每次正常橘子加入队列更新矩阵情况，并将正常橘子的个数减一
7. 每历遍完一层，更新需要的时间，自加
8. 返回答案，如果正常橘子全军覆没，返回污染时间，没有全军覆没，返回1

## Talk is cheap . Show me the code .

```c++
class Solution {
public:
    int x[4]={1,0,-1,0};
    int y[4]={0,1,0,-1};
    int orangesRotting(vector<vector<int>>& grid) {
        int good_cnt=0,min=-1,size=0;
        queue<pair<int,int>> bad;
        pair<int,int> temp;
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[i].size();j++){
                if(grid[i][j]==2){
                    temp=make_pair(i,j);
                    bad.push(temp);
                }else if(grid[i][j]==1) good_cnt++;
            }
        }
        if(good_cnt==0) return 0;
        while(!bad.empty()){
            size=bad.size();
            while(size--){
                temp=bad.front();bad.pop();
                for(int i=0;i<4;i++){
                    if(temp.first+x[i]>=0&&temp.first+x[i]<grid.size()&&temp.second+y[i]>=0&&temp.second+y[i]<grid[temp.first].size()){
                        if(grid[temp.first+x[i]][temp.second+y[i]]==1){
                            pair<int,int> point(temp.first+x[i],temp.second+y[i]);
                            bad.push(point);
                            grid[temp.first+x[i]][temp.second+y[i]]=2;
                            good_cnt--;
                        }
                    }
                }
            }
            min++;
        }
        return good_cnt==0?min:-1;
    }
};
```

