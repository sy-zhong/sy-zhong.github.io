---
title: 轮廓线dp入门题 && POJ - 2411
date: 2018-8-29
tags:
    - acm
    - dp
    - 状压dp
    - 轮廓线dp
---

题目很有意思，就是oj太烂了
刘汝佳的代码太优雅了，看不懂，但意思差不多。
就我做过的状压dp总是以一行（或一列）为一个状态，先理清一行中的关系，后再找行对行的关系
这里不能用行，行之间的关系不够了，因为要考虑不同的放法。
那一行不够，两行够不够？我觉得够，但时间复杂度不够优秀，会包含很多无效转态所以优化一下？
<!--more-->
<img src="https://images2015.cnblogs.com/blog/777259/201609/777259-20160906183914285-1571780409.png" width="300" hegiht="213" align=center />

dp三个问题：
1.状态的确立
2.状态转移方程
3.初始化

1.dp[i][j][state] : 右下角位置是（i，j），状态是state的总数
因为只要求dp[n-1][m-1][$2^m-1$]的值
滚动数组 为 dp[2][state] 
当前状态只于之前有关
2.转移方程:
若dp[cur][state]合理，那么转移到另一个合理状态。
如何转移
1.（上）考虑当前放竖着的块。
前提:上方的状态为0（没放）并且 现在不是第一行
dp[cur][可以到的状态]+=dp[1^cur][state]
2.（不放）考虑当前没有以（i，j）为右下角的块
前提:上方是1，（上方不能为0吧）
dp[cur][可以到的状态]+=dp[1^cur][state]
3.（左）。。。。差不多
```c++
#include<iostream>
#include<string>
#include<cstring>
#include<stdio.h>
#include<algorithm>
using namespace std;
#define mp(x, y) make_pair(x, y)
#define pb(x) push_back(x)
#define X first
#define Y second
#define fastin                    \
    ios_base::sync_with_stdio(0); \
    cin.tie(0);
#define clr(a, x) memset(a, x, sizeof(a))
typedef long long LL;
typedef pair<int, int> PII;
const int INF=0x3f3f3f3f;
int n,m,cur;
const int N =15;
LL dp[2][1<<N];

int main()
{
    while(scanf("%d%d",&n,&m)&&(n+m)){
        if(n<m) swap(n,m);
        clr(dp,0);
        cur=0;
        dp[0][(1<<m)-1]=1;
        for(int i=0;i<n;i++)
        for(int j=0;j<m;j++){
            cur^=1;
            clr(dp[cur],0);
            for(int s=0;s<(1<<m);s++){
                if(dp[1-cur][s]==0) continue;
                //上方
                if(i!=0 && (s&(1<<(m-1)))==0) //非首行且上方是0 现在可以放竖着的块
                {
                    int now = ((s<<1)|1) & ((1<<m)-1);
                    dp[cur][now] += dp[1-cur][s];
                }
                //左方
                if( j!=0 && (s &(1<<(m-1))) && (s&1)==0 )
                {
                    int now=((s<<1)|3)&((1<<m)-1);
                    dp[cur][now]+=dp[1-cur][s];
                }
                //不放
                if(s &(1<<(m-1))){
                    int now=((s<<1)&((1<<m)-1));
                    dp[cur][now]+=dp[1-cur][s];
                }
            }
        }
        printf("%lld\n",dp[cur][(1<<m)-1]);
    }
    return 0;
}

```