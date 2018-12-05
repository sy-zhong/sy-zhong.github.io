---
title: poj 1821 Fence (dp+单调队列优化)
date: 2018-9-04
tags:
    - acm
    - dp
    - 单调队列
---

给n个篱笆，k个工人
接着给k行，分别描述工人的  **负责区域大小**  **涂一个篱笆的收益** **他所站的位置**
解释一下，一个工人如果涂篱笆了，一定是从他所站的位置出发的，即他负责的区域必须包括自己当前位置
求最大的总收益
<!--more-->
dp[i][j] : 前i个工人 负责前j个篱笆的的最大收益

dp[i][j]=max(dp[i][j-1],  dp[i-1][j-1],  **max(dp[i-1][k]+cost[i]*(j-k))** )

详细解释一下 dp[i-1][k]+cost[i]*(j-k)
前i-1个人负责前k个，剩下（j-k）个篱笆全由第i个人负责。
j-len<=k<=pos-1
有两个条件：
1.第i个人要能负责到第k+1个
2.j要大于等于其pos

程序分成两个部分：维护优先队列的部分，计算答案的部分

优先队列里维护什么值
维护 dp[i-1][j]-cost[i]*j 的最大值
```c++
#include<iostream>
#include<stdio.h>
#include<string>
#include<cstring>
#include<vector>
#include<stack>
#include<algorithm>
using namespace std;
#define clr(a, x) memset(a, x, sizeof(a))
#define mp(x, y) make_pair(x, y)
#define pb(x) push_back(x)
#define X first
#define Y second
#define fastin                    \
    ios_base::sync_with_stdio(0); \
    cin.tie(0);
typedef long long LL;
typedef pair<int, int> PII;
typedef vector<int> VI;
struct node{
    int len,pos,cost;
    bool operator <(const node& a)const {
        return pos<a.pos;
    }
}a[110];
int dp[110][16050];
int q[16050];

int main()
{
    int n,k;
    while(~scanf("%d%d",&n,&k)){
        for(int i=1;i<=k;i++)
            scanf("%d%d%d",&a[i].len,&a[i].cost,&a[i].pos);
        sort(a+1,a+k+1);
        clr(dp,0);
        for(int i=1;i<=k;i++)
        {
            int h=0,t=0;
            q[t++] = max(0,a[i].pos-a[i].len);//找起点
            for(int j=1;j<=n;j++)
            {
                dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                if(j>=a[i].pos+a[i].len) continue;
                while(h<t &&q[h]+a[i].len < j) h++;//不合法
                if(j<a[i].pos){
                    int tmp=dp[i-1][j]-j*a[i].cost;
                    while(h<t && dp[i-1][q[t-1]]-q[t-1]*a[i].cost < tmp) t--;
                    q[t++] = j;
                }
                else{
                    dp[i][j] = max(dp[i][j],dp[i-1][q[h]]+a[i].cost*(j-q[h]));
                }//这里不能入栈 k<=pos-1
            //cout<<i<<" "<<j<<" "<<dp[i][j]<<endl;
            //cout<<h<<" "<<t<<endl<<endl;
            }
        }
        printf("%d\n",dp[k][n]);
    }
    return 0;
}
/*
8 4
3 2 2
3 2 3
3 3 5
1 1 7

*/

```

*tip：取队列尾部元素的时候要减1