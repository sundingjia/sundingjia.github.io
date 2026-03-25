---
title: 近期SYOJ大规模比赛集中题解
date: 2026-02-23 17:29:00
tags: 题解
---

# 比赛题解

## week 1 

### A

#### 问题

![](/img/blog5/1A.png)

![](/img/blog5/1A1.png)

#### 前置知识

贪心，二分。

#### 解答

贪心。

假设 $B_j$ 对应 $A_i$，$B_1,B_2\dots B_j$ 我们都找好了所对应的 $A$。

现在考虑 $B_{j+1}$，我们贪心的靠前匹配，这样一定比往后匹配不劣。

每次二分找到 $A_i$ 之后第一个等于 $B_j$ 的值。

时间复杂度 $O(m \log n)$。

#### code

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e6+10;
vector<int> e[N];
int n,q,m;
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		int c;
		cin>>c;
		e[c].push_back(i);
	}
	for(int i=1;i<=1e6;i++) e[i].push_back(n+1);
	cin>>q;
	while(q--)
	{
		cin>>m;
		int pos=0,flag=0;
		for(int i=1;i<=m;i++)
		{
			int x;
			cin>>x;
			pos=*(upper_bound(e[x].begin(),e[x].end(),pos));
			// cout<<pos<<' ';
			if(pos>n)
			{
			      flag=1;
            pos--;
			}
		}
		if(!flag) cout<<"YES\n";
    else cout<<"NO\n";
	}
	return 0;
}
```

### B

在建

### C

在建

### D

在建

## week 2

### A

#### 问题

![](/img/blog5/2A.png)

![](/img/blog5/2A1.png)

#### 前置知识

区间 DP,Hash

#### 题解

$dp_{i,j}$ 表示区间 $[i,j]$ 最少需要多少个 `cout`。

$$dp_{i,j}=\min(j-i+1, \min_{i \le k < j} (dp_{i,k}+dp_{k+1,j}),\min _{k,[i,j]可由若干个[i,k]首尾相连而成}(\frac{k-i+1}{j-i+1}dp_{i,k}))$$

第三部分拿 Hash 就行了。

复杂度 $O(n^3 \log n)$,实际带点优化，很难跑满。

#### code

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 110;
int dp[N][N];
string s;
typedef unsigned long long ull;
ull h[N];
const int B = 13;
int n,m;
int a[N];
ull p[N];
ull geths(int l,int r)
{
	return h[r]-h[l-1]*p[r-l+1];
}
void solve()
{
	cin>>n>>m;
	for(int i=1;i<=n;i++)
	{
		cin>>a[i];
	}
	for(int i=1;i<=n;i++)
	{
		h[i]=h[i-1]*B+a[i];
	}
	for(int i=1;i<=n;i++) 
		for(int j=i;j<=n;j++)
			dp[i][j]=j-i+1;
	for(int i=2;i<=n;i++)
	{
		for(int j=1;j+i-1<=n;j++)
		{
			int k=j+i-1;
			for(int l=1;l<k;l++)
			{
				dp[j][k]=min(dp[j][k],dp[j][l]+dp[l+1][k]);
			}
			for(int l=1;l<i;l++)
			{
				bool flag=1;
				if(i%l==0)
				{
					
					for(int o=j;o<=k;o+=l)
					{
						if(geths(o,o+l-1)!=geths(j,j+l-1))
						{
							flag=0;
							break;
						}
					}
					if(flag)
					{
						dp[j][k]=min(dp[j][k],dp[j][j+l-1]);
					}
				}	
			}			
		}
	}
	if(dp[1][n]>m) cout<<"NO\n";
	else cout<<"YES\n";
}
int _;
int main()
{
	cin>>_;
	p[0]=1;
	for(int i=1;i<=100;i++)
	{
		p[i]=p[i-1]*B;
	}
	while(_--) solve();
	return 0;
}
```

