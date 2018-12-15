---
title: 青岛区域赛备战--模板及复习--dp
date: 2018-11-01 15:25:26
tags:
    - acm
    - 模板
---

# 青岛区域赛备战--模板及复习--dp

## 区间dp
```c++
for(.....)//枚举起点
for(.....)//枚举长度
		  //处理区间
或 dfs写法
dp方程随缘

```
<!--more-->

##  状压dp *
看清维数，弄清楚转移方向啊



## 数位dp
```c++
typedef long long LL;
int a[20];
LL dp[20][state];
LL dfs(int pos,/*state变量*/,bool limit/*数位上界变量*/)
{
    if(pos==-1) return 1;
    if(!limit && dp[pos][state]!=-1) return dp[pos][state];
    int up=limit?a[pos]:9;
    LL ans=0;
    for(int i=0;i<=up;i++)
    {
        if() ...
        else if()...
        ans+=dfs(pos-1,/*状态转移*/,limit && i==a[pos])
    }
    if(!limit ) dp[pos][state]=ans;
    return ans;
}


LL solve(LL x)
{
    int pos=0;
    while(x)
    {
        a[pos++]=x%10;
        x/=10;
    }
    return dfs(pos-1/*从最高位枚举*/, /*一系列状态 */ ,true);
}

int main()
{
    LL le,ri;
    memset(dp,-1,sizeof dp);
    while(~scanf("%lld%lld",&le,&ri))
    {
        printf("%lld\n",solve(ri)-solve(le-1));
    }
}
```

## 背包
```c++
//01背包
for(i=1;i<=N;i++){
	for(j=V;j>=c[i];j--){
		f[j]=max(f[j],f[j-c[i]]+w[i]);
	}
}

//完全背包
for(i=1;i<=N;i++){
	for(j=c[i];j<=V;j++){
		f[j]=max(f[j],f[j-c[i]]+w[i]);
	}
}

//多重背包 二进制优化
for(i=0;i<n;i++){
	scanf("%d%d%d",&wi,&vi,&c);
	for(j=1;j<=c;j<<=1){
		v.push_back(j*vi);
		w.push_back(j*wi);
		c=c-j;
	}
	if(c>0){
		v.push_back(c*vi);
		w.push_back(c*wi);
	}
}
for(i=0;i<v.size();i++){
	for(j=W;j>=w[i];j--){
		dp[j]=max(dp[j-w[i]]+v[i],dp[j]);
	}
}

//分组背包
for(int i=0;i<n;i++){//n是分组数 
	for(int j=V;j>=0;j--){//V是背包体积 
		for(int k=0;k<num[i];k++){//num[i]是第i组物品的数量 
			if(j>=w[i][k])dp[j]=max(dp[j-w[i][k]]+v[i][k],dp[j])//w[i][k]是第i组第k个物品的体积
		}
	}
} 

```

dp随缘