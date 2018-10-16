# HDU warm up

`桥` `边双` `树的直径`

### 题意

> 有一张联通的无向图,再加一条无向边后最少的桥的数量

- 求桥时将边双缩点,记录桥数 **ans**
- 建树,求最长路长度 **bns**
- 答案为 **ans-bns** 
- 多组数据,注意初始化

```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int maxn=2e5+10,maxm=1e6+10;
int n,m,head[maxn],num,cnt,bi,ans;
int dfn[maxn],low[maxn],input,fa[maxn],pa[maxn];
int xx[maxn],w,headd[maxn],numm,mdep,ww;
struct fy{int from,to,next,h;}q[maxm<<1];
struct ffy{int to,next;}qq[maxm<<1];
void add(int a,int b)
{
	q[++num]=(fy){a,b,head[a],++bi};head[a]=num;
	q[++num]=(fy){b,a,head[b],bi};head[b]=num;
}
void addd(int a,int b){qq[++numm]=(ffy){b,headd[a]};headd[a]=numm;}
void tar(int a)
{
	dfn[a]=low[a]=++input;xx[++w]=a;
	for(int i=head[a];i;i=q[i].next)
	{
		int b=q[i].to;if(q[i].h==pa[a])continue;
		if(!dfn[b])
		{
			pa[b]=q[i].h;tar(b);
			low[a]=min(low[a],low[b]);
			if(dfn[a]<low[b])ans++;
		}
		else low[a]=min(low[a],dfn[b]);
	}
	if(low[a]==dfn[a])
	{
		++cnt;
		while(xx[w+1]!=a)
		{
			fa[xx[w]]=cnt;
			w--;
		}
	}
}
void dfs(int a,int fat,int dep)
{
	if(dep>mdep)
	{
		mdep=dep;
		ww=a;
	}
	for(int i=headd[a];i;i=qq[i].next)
	{
		int b=qq[i].to;if(b==fat)continue;
		dfs(b,a,dep+1);
	}
}
int main()
{
	int a,b;
	while(scanf("%d%d",&n,&m)==2)
	{
		if(n==0&&m==0)break;
		memset(head,0,sizeof head);memset(dfn,0,sizeof dfn);
		memset(low,0,sizeof low);memset(pa,0,sizeof pa);
		memset(headd,0,sizeof headd);
		num=0;cnt=0;bi=0;w=0;input=0;numm=0;ans=0;ww=0;
		for(int i=1;i<=m;i++)
		{scanf("%d%d",&a,&b);add(a,b);}
		tar(1);
		for(int i=1;i<=num;i++)
		{
			a=fa[q[i].from];b=fa[q[i].to];
			if(a!=b)addd(a,b);
		}
		mdep=0;dfs(1,0,0);
		mdep=0;dfs(ww,0,0);
		printf("%d\n",ans-mdep);
	}
	return 0;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1OTc1ODc0MDddfQ==
-->