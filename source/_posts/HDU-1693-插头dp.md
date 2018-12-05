---
title: HDU - 1693 插头dp
date: 2018-9-02
tags:
    - acm
    - dp
    - 状压dp
    - 插头dp
---

什么是插头dp？
首先我们要先知道它能解决那些问题。一般都是一张方格图里有关连通性的一些问题。
那么对于一个格子而言，他就**可能**会有一些线经过它。（所以也可以没有，看题目）
<!--more-->
例如这张图
<img src="http://acm.hdu.edu.cn/data/images/C116-1005-1.JPG" width="300" hegiht="213" align=center />
那么**插头**就是**描述这些线条如何穿过这些格子**

<img src="http://hi.csdn.net/attachment/201109/7/0_1315370883shfd.gif" width="600" height="213" align=center />
那如何用插头来表示一种状态？
对于一个宽度为m的图，它的插头就有m+1种，每一种插头的01状态代表这个位置有没有插头。

那么状态如何转移呢？
![这里写图片描述](https://img-blog.csdn.net/2018090213155386?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d5eHh6c3k=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
就这么几种状态，可以分类讨论或者适当简化一下。

一行之尾与下一行之首间的转化
![这里写图片描述](https://img-blog.csdn.net/2018090213351713?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d5eHh6c3k=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

初始化的时候就是-1行state为0时才为1

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
int G[15][15];
LL dp[2][(1<<12)+10];
int n,m;
int main()
{
    int t;scanf("%d",&t);
    int cas=0;
    while(t--){
        scanf("%d%d",&n,&m);
        for(int i=0;i<n;i++)
            for(int j=0;j<m;j++)
            scanf("%d",&G[i][j]);
        int total=1<<(m+1);
        //初始化
        clr(dp,0);
        //至于其他初值取决于行之间的转移
        dp[1][0]=1;
        int cur=1;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){//对
                int plug1=(1<<j),plug2=(1<<(j+1));
                cur^=1;
                clr(dp[cur],0);
                for(int st=0; st<total; st++){
                    if(G[i][j]){
                        dp[cur][st^plug1^plug2] += dp[cur^1][st];
                        if(((plug1&st)==0)^((plug2&st)==0)) dp[cur][st]+=dp[cur^1][st];
                    }
                    else {
                        if((plug1&st)||(plug2&st)) dp[cur][st]=0;
                        else dp[cur][st] += dp[cur^1][st];
                    }
                }
            }
            cur^=1;
            clr(dp[cur],0);
            for(int st=0;st<(1<<m);st++)
                dp[cur][(st<<1)] += dp[cur^1][st];
        }
        printf("Case %d: There are %lld ways to eat the trees.\n",++cas,dp[cur][0]);
    }
    return 0;
}
```