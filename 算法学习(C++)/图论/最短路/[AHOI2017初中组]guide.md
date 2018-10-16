# [AHOI2017初中组]guide

`最短路`

### 题意
> 对于一张有向图,每条边有两个边权,有两套GPS会分别按两个边权计算所在到终点的最短路,如果你不按其中一套GPS说的路走,就会产生1的抱怨,求从起点到终点的最少抱怨数

### 题解
> 因为每次两套GPS都会判断当前要去的点是否在当前点到终点的最短路上,所以我们可以反向建边,跑出每套GPS从终点到每个点的最短路,然后我们重新建图,依次枚举原图的每条边,将边权赋值为2,如果要去的点在某套GPS的最短路上,边权减一.最后跑一遍最短路就可以得出$ans$

- 反向建边
- 反向SPFA
- 边重构
- 反向SPFA
- $ans$

```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<queue>
using namespace std;
const int maxn=1e5+10;
int n,m,head[maxn],num;
int dis[maxn],dis1[maxn],di[maxn];
bool use[maxn];
struct fy{int from,to,d,dd,e,next;}q[maxn*5];
void add(int a,int b,int c,int d)
{
	q[++num]=(fy){a,b,c,d,0,head[a]};head[a]=num;
}
void sp()
{
	memset(dis,0x3f,sizeof dis);dis[n]=0;
	memset(dis1,0x3f,sizeof dis1);dis1[n]=0;
	queue<int>qq;qq.push(n);use[n]=true;
	while(!qq.empty())
	{
		int a=qq.front();qq.pop();use[a]=false;
		for(int i=head[a];i;i=q[i].next)
		{
			int b=q[i].to;
			if(dis[b]>dis[a]+q[i].d)
			{
				dis[b]=dis[a]+q[i].d;
				if(!use[b]){use[b]=true;qq.push(b);}
			}
		}
	}
	qq.push(n);use[n]=true;
	while(!qq.empty())
	{
		int a=qq.front();qq.pop();use[a]=false;
		for(int i=head[a];i;i=q[i].next)
		{
			int b=q[i].to;
			if(dis1[b]>dis1[a]+q[i].dd)
			{
				dis1[b]=dis1[a]+q[i].dd;
				if(!use[b]){use[b]=true;qq.push(b);}
			}
		}
	}
}
void sp1()
{
	memset(di,0x3f,sizeof di);di[n]=0;
	queue<int>qq;qq.push(n);use[n]=true;
	while(!qq.empty())
	{
		int a=qq.front();qq.pop();use[a]=false;
		for(int i=head[a];i;i=q[i].next)
		{
			int b=q[i].to;
			if(di[b]>di[a]+q[i].e)
			{
				di[b]=di[a]+q[i].e;
				if(!use[b]){use[b]=true;qq.push(b);}
			}
		}
	}
}
int main()
{
	scanf("%d%d",&n,&m);int a,b,c,d;
	for(int i=1;i<=m;i++)
	{
		scanf("%d%d%d%d",&a,&b,&c,&d);
		add(b,a,c,d);
	}
	sp();//反向SPFA
	for(int i=1;i<=num;i++)//边重构
	{
		d=2;a=q[i].from;b=q[i].to;
		if(dis[b]==dis[a]+q[i].d)d--;
		if(dis1[b]==dis1[a]+q[i].dd)d--;
		q[i].e=d;
	}
	sp1();
	printf("%d\n",di[1]);
	return 0;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM3NTA2Nzg0NV19
-->