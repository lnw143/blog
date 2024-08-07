---
authors:
  - lnw143
date:
  created: 2024-08-07
  updated: 2024-08-07
categories:
  - String
---

# 后缀数组

后缀数组是处理字符串的利器。

## 定义

对于长度为 $n$ 的字符串 $s = s_1 s_2 \cdots s_n$，定义 $s[i,j] = s_i s_{i+1} \cdots s_j$，即 $s$ 的第 $i$ 个到第 $j$ 个字符形成的子串。

定义 `sa` 为 $s$ 的后缀数组，`sa[i]` 表示第 $i$ 小的后缀的编号。

定义 `rk[sa[i]]=i`，即 `rk[i]` 表示后缀 $i$ 的 rank。

## 求解

考虑最暴力的做法，取出所有后缀并排序，空间复杂度 $O(n ^ 2)$，时间复杂度 $O(n^2 \log n)$。

考虑倍增，设 `rk[j][i]` 表示在所有长度为 $2^j$ 的后缀中，后缀 $s[i,i+2^j-1]$ 的 rank（超出 $n$ 的部分记为空字符，小于任何非空字符）。

显然我们可以 naively 得到 `rk[0][i]`，考虑如何由 `rk[j-1]` 得到 `rk[j]`。

发现可以将所有二元组 `(rk[j-1][i],rk[j-1][i+(1<<(j-1))]` 排序，即把每段后缀分为前后两段，并以前一半的 rank 作为第一关键字，后一半的 rank 作为第二关键字，超出 $n$ 的部分记为 `0`，排序后得到 `rk[j]`。

总共倍增 $\mathrm O(\log n)$ 次，每次排序 $\mathrm O(n \log n)$，总时间复杂度 $\mathrm O(n \log^2 n)$。

发现 `rk` 的值域是 $\mathrm O(n)$ 的，因此可以用桶排做到 $O(n \log n)$。

在算法竞赛中这已经足够了，如果追求更优的时间复杂度，可以考虑 SA-IS / DC3 算法，线性时间复杂度，但实现十分复杂，在竞赛中不常用，因此不再赘述。

显然通过 `rk` 可以 $\mathrm O(n)$ 得到 `sa`。

## height 数组

定义 `ht[i]` 为后缀 `sa[i]` 与 `sa[i-1]` 的最长公共前缀的长度，并设 `ht[1]=0`。

一眼看来似乎没有线性做法，但是我们将尝试证明以下引理：

*Lemma:* `ht[rk[i]]` $\ge$ `ht[rk[i-1]]-1`。

---

*Proof:*

`ht[rk[i-1]]` $\le 1$ 的情况是 naive 的。

若 `ht[rk[i-1]]` $\ge 2$，即 `sa[rk[i-1]-1]` 与 `i-1` 的最长公共前缀长度 $\ge 2$，设这个最长公共前缀为 $aA$，其中 $a$ 是单个字符，$A$ 是一个非空字符串。

那么设后缀 `sa[rk[i-1]-1]` 为 $aAB$，`i-1` 为 $aAC$，则 $B \lt C$，且后缀 `sa[rk[i-1]-1]+1` 为 $AB$，`i` 为 $AC$。

后缀 `sa[rk[i]-1]` 因为只比 `i` 前一名，所以是小于 `i` 中最大的，又因为 $AB \lt AC$，所以 $AB \le$ 后缀 `sa[rk[i]-1]` $\lt AC$，容易看出后缀 `i` 和后缀 `sa[rk[i]-1]` 有公共前缀 $A$。

于是 `ht[rk[i]]` $\ge |A| = $ `ht[rk[i-1]]-1`。

$\square$

---

由此，我们从 `ht[rk[i-1]]-1` 开始暴力拓展 `ht[rk[i]]`，在 $\mathrm O(n)$ 的时间复杂度内得到 `ht`。

`ht` 数组有这样一个性质：后缀 `i,j` 的最长公共前缀长度等于 `ht[rk[i],rk[j])` 中的最小值。（钦定 `rk[i]` $\lt$ `rk[j]`）

因此我们可以用 ST 表 $\mathrm O(n \log n)$ 预处理后 $\mathrm O(1)$ 询问任意后缀的最长公共前缀长度。

## 例题

??? note "[UOJ 35.后缀排序](https://uoj.ac/problem/35)"
    ```cpp
    #include <cstdio>
	#include <algorithm>
	#include <cstring>
	using namespace std;
	const int N = 1e5;
	int n,sa[N + 2],cnt[N + 2],rk[2][N + 2],ht[N + 2];
	char s[N + 2];
	struct node {
		int x,y,id;
	} p[N + 2],q[N + 2];
	bool operator<(const node& a,const node& b) {
		return a.x!=b.x?a.x<b.x:a.y<b.y;
	}
	bool operator==(const node& a,const node& b) {
		return a.x==b.x&&a.y==b.y;
	}
	void Sort() {
		memset(cnt,0,sizeof(*cnt)*(n+1));
		for(int i=1; i<=n; ++i) ++cnt[p[i].y];
		for(int i=1; i<=n; ++i) cnt[i]+=cnt[i-1];
		for(int i=n; i>=1; --i) q[cnt[p[i].y]--]=p[i];
		memset(cnt,0,sizeof(*cnt)*(n+1));
		for(int i=1; i<=n; ++i) ++cnt[q[i].x];
		for(int i=1; i<=n; ++i) cnt[i]+=cnt[i-1];
		for(int i=n; i>=1; --i) p[cnt[q[i].x]--]=q[i];
	}
	int main() {
		scanf("%s",s+1);
		n=strlen(s+1);
		for(int i=1; i<=n; ++i) cnt[s[i]]=1;
		for(int i=1; i<128; ++i) cnt[i]+=cnt[i-1];
		for(int i=1; i<=n; ++i) rk[0][i]=cnt[s[i]];
		for(int j=1,k=1; j<=n; j<<=1,k^=1) {
			for(int i=1; i<=n; ++i) p[i]={rk[k^1][i],i+j<=n?rk[k^1][i+j]:0,i};
			Sort();
			for(int i=1,t=0; i<=n; ++i)
				if(i>1&&p[i]==p[i-1])
					rk[k][p[i].id]=t;
				else
					rk[k][p[i].id]=++t;
			if((j<<1)>n) swap(rk[k],rk[0]);
		}
		for(int i=1; i<=n; ++i) sa[rk[0][i]]=i;
		for(int i=1; i<=n; ++i) printf("%d ",sa[i]);
		putchar('\n');
		for(int i=1; i<=n; ++i) {
			if(rk[0][i]==1) continue;
			int k=i>1?max(0,ht[rk[0][i-1]]-1):0;
			while(i+k<=n&&sa[rk[0][i]-1]+k<=n&&s[i+k]==s[sa[rk[0][i]-1]+k]) ++k;
			ht[rk[0][i]]=k;
		}
		for(int i=2; i<=n; ++i) printf("%d ",ht[i]);
		return 0;
	}
	```

