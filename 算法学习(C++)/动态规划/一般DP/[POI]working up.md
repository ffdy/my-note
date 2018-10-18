---
title: [POI]working up
tags: DP
categories: 算法学习
top: false

---
# [POI]working up
### 题意
> 有两个人和一个$n \times m$的矩阵,一个从左上角出发,只能向右,向下走,去右下角;另一个从右上角出发只能向左,向下走,去左下角.矩阵的每一格有一个权值,走过会得到这个权值.很显然,两个人肯定会相遇,他们相遇的那一格的权值不取,求两人只相遇一次权值和的最大值

### 题解
> **读完题,你是否有想到`方格取数`
> 做法非常简单也非常暴力,开四个二维数组分别记录从四个角出发的最优解,然后$n^2$枚举合法的点,更新$ans$
> 因为只相遇一次,所以两人相遇时只有两种直线走法,不会转弯**
- 注意下标,容易迷糊
- 记得开`long long`
### Coding:
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
#define ll long long
using namespace std;
const int maxn=1010;
ll mp[maxn][maxn],f1[maxn][maxn];
ll f2[maxn][maxn],f3[maxn][maxn];
ll f4[maxn][maxn];
int n,m;
int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++)for(int j=1;j<=m;j++)scanf("%lld",&mp[i][j]);
	for(int i=1;i<=n;i++)for(int j=1;j<=m;j++)
	f1[i][j]=max(f1[i-1][j],f1[i][j-1])+mp[i][j];
	for(int i=n;i;i--)for(int j=m;j;j--)
	f2[i][j]=max(f2[i+1][j],f2[i][j+1])+mp[i][j];
	for(int i=1;i<=n;i++)for(int j=m;j;j--)
	f3[i][j]=max(f3[i-1][j],f3[i][j+1])+mp[i][j];
	for(int i=n;i;i--)for(int j=1;j<=m;j++)
	f4[i][j]=max(f4[i+1][j],f4[i][j-1])+mp[i][j];
	//暴力美学
	ll ans=0;
	for(int i=2;i<n;i++)for(int j=2;j<m;j++)
	{
		ans=max(ans,f1[i][j-1]+f2[i][j+1]+f3[i-1][j]+f4[i+1][j]);
		ans=max(ans,f1[i-1][j]+f2[i+1][j]+f3[i][j+1]+f4[i][j-1]);
	}
	printf("%lld\n",ans);
	return 0;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMTI4NDkzMjAsODMxMTI3NjM4XX0=
-->