---
title: CodeForces - 11D
date: 2018-8-23 
tags:
    - acm
    - dp
    - 状压dp
---

有意思 和之前的项链异曲同工，但之前就是入门，半抄半做的。
one more time 再跑一遍吧
<!--more-->
环？怎么处理环？
都是通过记录路径，若是首末可相连，那么就是个环，同时，这样也可以用作之后的状态转移，妙啊
dp[i][s]: 路径的最后一个点是i且状态是s，且有隐含条件开头是是状态的第一个1。
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
#define legal(a,b) a&b

typedef long long LL;
typedef pair<int, int> PII;
typedef vector<int> VI;
const int INF=0x3f3f3f3f;
int G[21][21];
LL dp[21][1<<20];//dp[i][state] 以第一个1出现的位置作为开头，以i结尾的路径的数量
int num[1<<21];
int n,m;
//dp[i][s|(1<<i)] += dp[e][s]
int solve(int state)
{
    int ans=0;
    for(int i=0; i<=20; i++)
        if(state &(1<<i)) ans++;
    return ans;
}
int main()
{
    for(int i=0; i<(1<<21); i++)   num[i] = solve(i);

    while(cin>>n>>m)
    {
        clr(G,0);
        for(int i=0,x,y; i<m; i++)
        {
            cin>>x>>y;
            x--;  y--;
            G[x][y]=G[y][x]=1;
        }
        clr(dp,0);
        //初始化
        for(int i=0; i<n; i++)
            dp[i][1<<i] = 1;
        int total=1<<n;
        LL ans=0;
        for(int s=1; s<total; s++)
        {
            //找开头
            int st=0;
            for(int i=0; i<n; i++)
                if(s&(1<<i)) st=i,i=n;
            for(int i=st; i<n; i++) //枚举状态中的结尾
                if(s&(1<<i)&&(dp[i][s]))
                        for(int j=st; j<n; j++) //枚举新状态的结尾
                        {
                            if(s&(1<<j)) continue;
                            if(G[i][j]==0) continue;
                            dp[j][s|(1<<j)] += dp[i][s];
                            if(G[st][j] && num[s|(1<<j)]>=3)
                                ans+=dp[i][s];
                        }

        }
        cout<<ans/2<<endl;
    }
    return 0;
}
/*
4 6
1 2
1 3
1 4
2 3
2 4
3 4

*/
```
