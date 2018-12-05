---
title: LCS
date: 2018-7-16
tags:
    - acm
---

给出两个串 ，输出最长公共子序列
学习地址：https://blog.csdn.net/zhijianshafeiyang/article/details/45034853

“用一个数组将第一个串内元素在第二个串内的位置保存下来，求这个数组的最长上升子序列长度”
<!--more-->
什么叫第一个串内元素在第二个串内的位置？
比如 str1 = avbasx
     str2 = abcaszx

位置是反序 就是 4，1——2——4，1——5——7
eg2：   aaba
        abaa

4 3 1 —— 4 3 1—— 2 —— 4 3 1 

两个问题 1.记录位置
        2.位置的反序

```c++
#include<bits/stdc++.h>
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
const int MAX = 100000;
//---------------
int ans[MAX];
vector<int> a;
int LIS()
{
    int len=1;
    ans[0]=a[0];
    for(int i=1;i<a.size();i++){
        if(a[i]>ans[len-1]) ans[len++] = a[i];
        else
            *lower_bound(ans,ans+len,a[i]) = a[i];
    }
    return len;
}
//--
int a1[MAX],a2[MAX];
int LCSlen1,LCSlen2;
int LCS(){
    map<int,int> pos;
    a.clear();
    for(int i=0;i<LCSlen1;i++){
        pos[a1[i]] = i;
    }
    for(int i=0;i<LCSlen2;i++){
        if(pos.find(a2[i])!=pos.end())
            a.pb(pos[a2[i]]);
    }
    return LIS();
}
```

区域赛更新
```c++
int a[100100],ans[100100];
int LIS(int n){
    ans[0]=a[0];
    int len=1;
    for(int i=1;i<n;i++){
        if(a[i]>ans[len-1])
            ans[len++]=a[i];
        else{
            int pos=lower_bound(ans,ans+len,a[i])-ans; //在答案里找第一个比a[i]大的位置
            ans[pos]=a[i];
        }
    }
    return len;
}
```