---
title: kuangbin专题 搜索
date: 2018-7-18
tags: 
    - acm
    - 搜索
    - dfs
    - bfs
categories:
    - 搜索
---

## E
醉了，说有100位的十进制数，我想那会炸的啊，没想到啊，所有的答案都在long long范围内，那时候应该打个表看看

<!--more-->

## F
bfs 水题
## G
模拟题 怎么就搜索了？？没做
## H
bfs + 记录路径
怎么记录路径？记录每个点的前驱，像并查集那样向上跳，就是所求的路径。
但这里有一个问题，它要准确打印操作是什么
（我猜）就两个解决方法
1：记录下来  就结构体里多加点东西咯
2：打印答案路径的时候，对比v与前驱pre[v]，判断是什么
我这里用了第二种方法
```c++
#include<iostream>
#include<stdio.h>
#include<vector>
#include<cstring>
#include<map>
#include<queue>
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
const int INF = 0x3f3f3f3f;
const double eps = 1e-6;
typedef pair<int, int> PII;

int vol1,vol2,n;
map <PII,int > vis;
map <PII,PII> pre;
int num;

void ok(){
    cout<<"ok"<<endl;
}
void dfs(PII tmp)
{
    if(pre.find(tmp)!=pre.end())
    {
        num++;
        dfs(pre[tmp]);
        int u1=tmp.X,v1=tmp.Y;
        int u2=pre[tmp].X,v2=pre[tmp].Y;
        if(u1+v1==u2+v2){
            cout<<"POUR(";
            if(u1>u2) cout<<"2,1";
            else cout<<"1,2";
            cout<<")"<<endl;
        }
        else if(u1==vol1&&u2!=vol1)
            cout<<"FILL(1)"<<endl;
        else if(v1==vol2&&v2!=vol2)
            cout<<"FILL(2)"<<endl;
        else if(u1==0&&u2!=0)
            cout<<"DROP(1)"<<endl;
        else if(v1==0&&v2!=0)
            cout<<"DROP(2)"<<endl;

    }
    else{
        cout<<num<<endl;
    }
}


int main()
{
    while(cin>>vol1>>vol2>>n)
    {
        vis.clear();
        queue<PII> q;
        q.push(PII(0,0));
        int finish=0;
        while(!q.empty())
        {
            PII tmp=q.front();
            vis[tmp] = 1;
            q.pop();
            int u=tmp.X,v=tmp.Y;
            if(u==n||v==n) {
                num=0;
                dfs(tmp);
                finish=1;
                break;
            }
            //三个操作 六种情况
            int uu,vv;
            uu=vol1,vv=v;
            if(!vis[PII(uu,vv)]){q.push(PII(uu,vv));pre[PII(uu,vv)] = tmp;}

            uu=u,vv=vol2;
            if(!vis[PII(uu,vv)]){q.push(PII(uu,vv));pre[PII(uu,vv)] = tmp;}

            uu=0,vv=v;
            if(!vis[PII(uu,vv)]){q.push(PII(uu,vv));pre[PII(uu,vv)] = tmp;}

            uu=u,vv=0;
            if(!vis[PII(uu,vv)]){q.push(PII(uu,vv));pre[PII(uu,vv)] = tmp;}

            uu=min((u+v),vol1),vv=u+v-uu;
            if(!vis[PII(uu,vv)]){q.push(PII(uu,vv));pre[PII(uu,vv)] = tmp;}

            vv=min((u+v),vol2),uu=u+v-vv;
            if(!vis[PII(uu,vv)]){q.push(PII(uu,vv));pre[PII(uu,vv)] = tmp;}
        }
        if(!finish) {puts("impossible");continue;}

    }
    return 0;
}
```


## I - Fire Game
 FZU - 2150 
又一个神奇的oj 没c++11我写的好不习惯。
不过这题还不错，让我折腾了好久。
几个点：
1.永远注意数据范围
虽然数据小不意味着就要暴力，但这道题太tm小了，想用巧妙的方法做错了啊
2.要用vis数组的话，push前就更新比较保险
3.bfs别忘了pop，虽然总能debug出来，但好浪费时间啊，你就给我注意一下啊
题本身倒没什么，暴力跑bfs就可以了，吐槽一下 n^6我tm写了四个循环，这怎么看都要t了，奈何不住数据小啊
tips：看到一个优化
在枚举的时候 第三重和第四重可以不用从0枚举
```c++
for(int ii=0;ii<n;ii++)                       ==》  for(int ii=i;ii<n;ii++) 
for(int jj=0;jj<m;jj++)                      ==》  for(int jj=i==ii?j:0;jj<m;jj++)
```
看大佬的代码 至于为什么。。。，容我三思

额，其实就是
![1](1.jpg)
因为如果每行从自己开始的话会漏掉前面的一些点 或者干脆只优化一行，时间也没差多少
![2](2.jpg)
```c++
#include<iostream>
#include<stdio.h>
#include<vector>
#include<cstring>
#include<queue>
using namespace std;
#define clr(a, x) memset(a, x, sizeof(a))
#define mp(x, y) make_pair(x, y)
#define pb(x) push_back(x)
#define X first
#define Y second
#define fastin                    \
    ios_base::sync_with_stdio(0); \
    cin.tie(0);
typedef unsigned long long LL;
typedef pair<int,int> PII;
const int INF = 0x3f3f3f3f;
const double eps = 1e-6;
int n,m;
char G[20][20];
int vis[20][20];
int dis[20][20];
int res;
int dx[4]={0,0,1,-1};
int dy[4]={1,-1,0,0};

inline bool check(int x,int y)
{
    return x>=0&&y>=0&&x<n&&y<m&&G[x][y]=='#';
}

void bfs(int x1,int y1,int x2,int y2){
    queue<PII> q;
    clr(vis,0);
    clr(dis,0);
    q.push(PII(x1,y1));vis[x1][y1]=1;
    q.push(PII(x2,y2));vis[x2][y2]=1;
    int ans=0;
    while(!q.empty()){
        PII u=q.front();
        q.pop();
        ans=dis[u.X][u.Y];
        for(int i=0;i<4;i++){
            int xx=u.X+dx[i],yy=u.Y+dy[i];
            if(check(xx,yy)&&!vis[xx][yy])
            {
                vis[xx][yy]=1;
                dis[xx][yy]=ans+1;
                q.push(PII(xx,yy));
            }
        }
    }
    for(int i=0;i<n;i++)
        for(int j=0;j<m;j++)
        if(G[i][j]=='#'&&!vis[i][j]) return ;
    res=min(res,ans);
}


int main(){
    int t;
    scanf("%d",&t);
    for(int k=1;k<=t;k++){
        scanf("%d%d",&n,&m);
        for(int i=0;i<n;i++) scanf("%s",G[i]);
        res=INF;
        for(int i=0;i<n;i++)
        for(int j=0;j<m;j++)
        if(G[i][j]=='#')
        for(int ii=0;ii<n;ii++)
        for(int jj=0;jj<m;jj++)
        if(G[ii][jj]=='#')
        {
            bfs(i,j,ii,jj);
        }

        if(res==INF) res=-1;
        printf("Case %d: %d\n",k,res);
    }

}
```