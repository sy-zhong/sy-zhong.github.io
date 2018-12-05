---
title: dp专题总结
date: 2018-8-19 21:26:03
tags:
    - dp
mathjax: true

---


# dp专题总结
***
所有的dp关键有两点
1.看出来这是一道dp题 （看时间复杂度）
2.**状态转移方程！！！！**
其中状态的确立和推出状态转移方程是个难点,而且dp题还经常会和其他知识点融合在一起搞你，非常灵活。
先从有迹可循的一些经典dp问题入手

<!--more-->
## 一.数位dp

### 1.确立状态
如何确立一个正确的dp数组？
dp[pos][state1][state2][....]
首先第一维代表着数位
若是人为规定的话： 0-个位 1-十位 2-百位 3……
那么后面的state代表的就是 **所有的的i位数满足的性质**
eg. 如果要记录所有四的倍数的数量 （当然有更简单的容斥做法，这里只是举个例子）
开 dp[pos][mod] 
那么**dp[3][2]**代表着**所有千位数（0000-9999）**中%4余2的个数

那怎么确定我们要开那些状态？
1.题目中明确要求的
2.影响状态转移的（前导0什么的）
例如
>http://acmoj.shu.edu.cn/problem/65/
![这里写图片描述](https://img-blog.csdn.net/20180819144721784?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d5eHh6c3k=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
每组数据只有一行，包含三个整数 L_i,R_i,m。
在 [L_i,R_i ] 区间，有多少个数奇偶和等于 m，以及这些数的和（对和取模100000007后输出）。

和
>http://hihocoder.com/problemset/problem/1033
描述
给定一个数 x，设它十进制展从高位到低位上的数位依次是 a0, a1, ..., an - 1，定义交错和函数：
f(x) = a0 - a1 + a2 - ... + ( - 1)n - 1an - 1
例如：
f(3214567) = 3 - 2 + 1 - 4 + 5 - 6 + 7 = 4
给定 l, r, k，求在 [l, r] 区间中，所有 f(x) = k 的 x 的和，即：
![这里写图片描述](https://img-blog.csdn.net/2018081914512436?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d5eHh6c3k=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这两道题非常像，但是不同的地方导致我们的状态方程也不一样。
先考虑第一道题:
对于pos位,我们想要得到答案dp[pos][sum] 只需从dp[pos-1][sum-sgn*i]转移而来，用代码来表示的话，就是：
```c++
	for(int i=0; i<=up; i++)
    {
		int val;
        if(i&1) val=i*(-1);
        else val=i;
        node tmp=dfs ( pos-1, m - val, limit && i == up);
        ans.num += tmp.num;
        ans.sum=(ans.sum+i*POW[pos]%mod*tmp.num%mod 
        +tmp.sum)%mod;
    }
```
dp[pos][sum]:pos位数中满足奇偶和是sum的个数与其总和    
我们只需开多开一维sum记录  pos位数中满足奇偶和是sum的个数
当前的加减与当前的i有关，与之前和之后的数无关，转移并不会冲突。

第二道题：
我们能否也和上一题一样也开  dp[pos][sum]呢？
那好，先来解释一下 如果只开两维 dp数组的意义：
pos位数满足交错和为sum时的个数和总和
乍一看没问题，但当你写状态转移的时候，就会发现状态该怎么转移？
状态没法转移，当前是加，那么下一位就是减，反之亦然。你没有办法区分当前是正是负 还是前导0，当前状态的不同会导致之后状态也不一样。
所以给每个数再加上个性质：当前位的符号
dp[pos][sum][sgn] 
sgn: 0-有前导0 ,1-正, -1 负
	```c++
	struct node{
	    LL num,sum;
	}dp[30][500][3]; //偏移量240
	//第三维代表lead
	int a[30];
	LL Pow[30];
	int k;
	//lead 0-有前导0 1-正 -1 负
	
	node dfs(int pos,bool limit,int lead,int sum)
	{
	    if(pos==-1){
	        return node{sum==0,0};
	    }
	    if(!limit&& dp[pos][sum+240][lead].num!=-1) return dp[pos][sum+240][lead];
	    node ans=node{0,0};
	    int up=limit?a[pos]:9;
	    for(int i=0;i<=up;i++)
	    {
	        //处理前导0
	        int sgn=1;
	        if (lead==0 && i==0) sgn=0;
	        else if(lead==0 && i!=0) sgn=1;
	        else  sgn=-lead;
	
	        node tmp=dfs(pos-1,limit&&i==up,sgn,sum-sgn*i);
	        LL num=tmp.num,sum=tmp.sum;
	
	        (ans.num+=tmp.num)%=mod;
	        ans.sum+=Pow[pos]*i %mod *num %mod+ sum;
	        ans.sum%=mod;
	    }
	    return limit?ans:dp[pos][sum+240][lead]=ans;
	}
	
	
	LL solve(LL x){
	    if(x==-1) return 0;
	    int pos=0;
	    while(x){
	        a[pos++] = x%10;
	        x/=10;
	    }
	    return dfs(pos-1,1,0,k).sum;
	}
	```
其他难以确立的状态还有很多，比如大都会的四维dp，比如各种模数，连续字串。
但最为关键的就是记住**dp[pos][state1][state2][...]**
每多一个状态，那么就是pos位的数进一步被细分，可以看做是**各种性质的交集**，怎么去分这个数，就是确立状态的关键。

### 2.状态的转移
其实状态的转移多多少少要在确立这个状态的时候一并考虑到了，毕竟状态要能正确的转移，我们才能说这个确立的状态是对的嘛。
而数位dp的转移是比较单纯的，因为多半就是从高位到地位枚举，从低位到高位转移，而我们一般都是用数位dp记录满足条件数的个数，转移比较简单，写出dp数组，一般转移就出来了。
那如果要记录所有满足条件数的总和，平方和，别的什么奇奇怪怪的东西呢？
额，目前就遇到过 和 与 平方和。
和：当前枚举的数×当前的位权×（要转移到的）低位的满足某性质的个数+（要转移到的）低位的满足某性质的个数
eg：$ \cdots x  \cdots $
令$x_1~x_n$都是满足条件的低一位的数
$ sum = （x+x_1）+(x+x_2)+ \cdots (x+x_n) =n*x + x_1+\cdots+x_n$
开个struct 记录num和sum

平方和：

$ ans = (x+x_1)^2 + (x+x_2)^2 + \cdots + (x+x_n)^2 $
 &emsp;&emsp; $ =n*x^2+2x(x_1+x_2+ \cdots +x_n)$


 所以要维护三个值：个数 总和 平方和  
 eg https://vjudge.net/contest/70324#problem/J

## 二概率dp（期望dp）
感觉都可以用数学简化，但数学功底不够，就用dp来凑