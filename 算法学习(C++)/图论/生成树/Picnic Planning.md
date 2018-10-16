# Picnic Planning

`生成树` `状压` `K限制生成树`
### 题意
> 矮人虽小却喜欢乘坐巨大的轿车，车大到能够装下不管多少矮人。某天，N(N≤20)个矮人打算到野外聚餐。为了集中到聚餐地点，矮人A 要么开车到矮人B 家中，留下自己的轿车在矮人B 家，然后乘坐B 的轿车同行；要么直接开车到聚餐地点，并将车停放在聚餐地。尽管矮人的家非常大，能够停放无数量轿车，可是聚餐地点却最多仅仅能停放K 辆轿车。给你一张加权无向图，描写叙述了N 个矮人的家和聚餐地点，求出全部矮人开车最短总路程

### 题解
> **题本身是一道$k$限制生成树裸题,但那东西太迷幻,不会.因为数据规模比较小,暴力能过,所以也没学.这里用的是状压$+Prim$.**
> **因为根节点只能连不大于$k$条边,所以我们暴力枚举根节点能连的边的状态,再将剩下的点跑最小生成树,更新最小代价.**

- 用$map$映射每个人的名字
- 可能有重边,需要更新两点之间的最短距离
- 这里我们用邻接矩阵存边,用$Prim$跑最小生成树,不然时间复杂度过不去
- 每次生成树必须加够$N-1$条边后才能更新$ans$
- 输出应遵循题目规则

```cpp
#include<cstdio>
#include<map>
#include<cstring>
#include<string>
#include<algorithm>
using namespace std;
const int maxn=430;
int n,k,num,cnt;
int fa[22],ans,bns;
map< string , int >mm;
int ss[22][22];
struct fy
{
	int from,to,d;
	bool operator<(const fy&a)const{return d<a.d;};
}q[maxn];
char str[30],str1[30];
void add(int a,int b,int c){q[++num]=(fy){a,b,c};}
int can(int a)
{
	bns=0;
	int he=0,w=1;
	while(a)
	{
		++w;
		if(a&1)
		{
			he++;fa[w]=1;
			if(ss[1][w]==ss[0][0])return 0;
			bns+=ss[1][w];
		}
		a>>=1;
	}
	if(he<=k)return he;
	return 0;
}
int find(int a)
{
	while(a!=fa[a])a=fa[a]=fa[fa[a]];
	return a;
}
void ku(int x)//披着羊皮的狼(滑稽)
{
	if(!x)return;
	for(int i=1;i<=num;i++)
	{
		if(q[i].from==1||q[i].to==1)continue;
		int a=find(q[i].from);
		int b=find(q[i].to);
		if(a!=b)
		{
			if(x==(cnt-1))break;
			fa[a]=b;
			x++;
			bns+=q[i].d;
			if(bns>ans)return;
		}
	}
	if(x==(cnt-1))ans=min(ans,bns);
}
int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		mm.clear();cnt=0;num=0;
		scanf("%d",&n);memset(ss,0x3f,sizeof ss);
		mm["Park"]=++cnt;ans=ss[0][0];
		for(int i=1;i<=n;i++)
		{
			int a,b,c;
			scanf("%s%s%d",str,str1,&c);
			if(!mm[str])mm[str]=++cnt;
			if(!mm[str1])mm[str1]=++cnt;
			a=mm[str];b=mm[str1];
			ss[a][b]=min(ss[a][b],c);
			ss[b][a]=ss[a][b];
		}
		scanf("%d",&k);
		for(int i=1;i<=cnt;i++)for(int j=i+1;j<=cnt;j++)
		if(ss[i][j]&&(ss[i][j]!=ss[0][0]))add(i,j,ss[i][j]);
		sort(q+1,q+1+num);
		for(int s=0;s<(1<<(cnt-1));s++)//枚举根节点的状态
		{
			for(int i=1;i<=cnt;i++)fa[i]=i;
			ku(can(s));
		}
		printf("Total miles driven: %d\n",ans);//输出
	}
	return 0;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNTg2NDcyMTddfQ==
-->