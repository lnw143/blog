---
authors:
  - lnw143
categories:
  - Number Theory
date: 2024-07-09
---

# Pollard-Rho 算法

## 推导

对于一个已知的合数 $n>3$，我们的目标是快速找到它的一个**非平凡因子**（除 $1$ 和 $n$ 外）。

显然问题可以转化为找到一个 $k$ 使得 $1 \lt \gcd(k,n) \lt n$。

考虑一个序列 $f$，满足 $f_0=0$ 且 $f_i = (f_{i-1}^2 + c) \bmod n$，其中 $c$ 是常数。由生日悖论[^1]得 $f$ 中不同的数量的期望为 $O(\sqrt n)$。

设 $m$ 是 $n$ 的最小质因子，显然有 $m \le \sqrt n$。

设 $g_i = f_i \bmod m$，则 $g$ 中不同的数量的期望为 $O(\sqrt m)$。

于是我们可以在期望 $O(\sqrt m) \le O(n ^ {\frac 1 4})$ 的时间内找到两个位置 $i,j$ 使得 $f_i \not = f_j \land g_i = g_j$，即 $n \nmid (f_i - f_j) \land m \mid (f_i - f_j)$，于是有 $1 \lt m \le \gcd(|f_i - f_j|, n) \lt n$

于是我们可以在期望 $O(n ^ {\frac 1 4})$ 的时间内找到 $k$ 满足 $1 \lt gcd(k,n) \lt n$，也就找到了一个 $n$ 的非平凡因子。

## 实现

我们随机 $c \in [1,n)$。

### Floyd 判环

由于 $f$ 的值域在 $[0,n)$ 之间，于是其必定有循环节，因为其值的排列图像形似 $\rho$，于是这个算法被称为 `Pollard-Rho`。

我们考虑枚举 $i$，判断是否 $1 \lt \gcd(|f_i - f_{2i}|,n) \lt n$，并在当 $f_i = f_{2i}$ 时终止。

??? note "code"
	=== "C++"
		```cpp
		ll pollard_rho(ll n) {
			const ll c = randint(1,n-1);
			const auto f = [&](ll x) -> ll {
				return (mul(x,x,n)+c)%n;
			};
			ll s=f(0),t=f(f(0));
			while(true) {
				if(s==t) break;
				ll d=gcd(abs(s-t),n);
				if(d>1) return d;
				s=f(s);
				t=f(f(t));
			}
			return n;
		}
		```

### 倍增思想

考虑设两个变量 $s,t$，我们让 $t$ 记下 $s$ 的目前值，并让 $s$ 跑一定距离，在这过程中检验 $|s-t|$，并每次都把距离翻倍，显然当 $s$ 跑到 $t$，即出现环时，$s$ 并没有多跑多远。

??? note "code"
	=== "C++"
		```cpp
		ll pollard_rho(ll n) {
			const ll c = randint(1,n-1);
			const auto f = [&](ll x) -> ll {
				return (mul(x,x,n)+c)%n;
			};
			ll s=0,t=0;
			for(int j=1; ; j<<=1,t=s)
				for(int i=1; i<=j; ++i) {
					s=f(s);
					if(s==t) return n;
					ll d=gcd(abs(s-t),n);
					if(d>1) return d;
				}
			return n;
		}
		```

### 基于倍增的二次优化

我们发现上面两种做法时间复杂度都不是单纯的 $O(n ^ {\frac 1 4})$，而是 $O(n ^ {\frac 1 4} \log n)$，因为还要调用 $\gcd$。

为了减小调用 $gcd$ 的次数，我们考虑若 $\gcd(a,b) \gt 1$，显然也有 $\gcd(ac \bmod b,b) \gt 1$，于是我们把一段路程放到一起检验。

根据前人的智慧，取段长为 $127$ 效果最好。

??? note "code"
	=== "C++"
		```cpp
		ll pollard_rho(ll n) {
			const ll c = randint(1,n-1);
			const auto f = [&](ll x) -> ll {
				return (mul(x,x,n)+c)%n;
			};
			ll s=0,t=0,v=1;
			for(int j=1; ; j<<=1,t=s,v=1) {
				for(int i=1; i<=j; ++i) {
					s=f(s);
					v=mul(v,abs(s-t),n);
					if(!v) return n;
					if(i%127==0) {
						ll d=gcd(v,n);
						if(d>1) return d;
					}
				}
				ll d=gcd(v,n);
				if(d>1) return d;
			}
			return n;
		}
		```

### 完整代码

??? note "[P4718 【模板】Pollard-Rho](https://www.luogu.com.cn/problem/P4718)"
	=== "C++"
		```cpp
		#include <cstdio>
		#include <random>
		#include <chrono>
		#include <algorithm>
		#include <cmath>
		using namespace std;
		using ll = long long;
		int T;
		ll n,maxf;
		mt19937 rnd(chrono::steady_clock::now().time_since_epoch().count());
		ll randint(ll l,ll r) {
			return uniform_int_distribution<ll>(l,r)(rnd);
		}
		ll mul(ll a,ll b,ll p) {
			ll c=1.0l*a/p*b;
			return (ll(1ull*a*b-1ull*c*p)+p)%p;
		}
		ll qpow(ll a,ll n,ll p) {
			a%=p;
			ll x=1;
			for(; n; n>>=1,a=mul(a,a,p)) if(n&1) x=mul(x,a,p);
			return x;
		}
		bool miller_rabin(ll n) {
			if(n<3) return n==2;
			if(n%2==0) return false;
			ll s=n-1,t=0;
			while(s%2==0) s/=2,++t;
			for(int i=0; i<8; ++i) {
				ll x=randint(2,n-1),u=qpow(x,s,n);
				if(u==1) continue;
				{
					int j;
					for(j=0; j<t; ++j) {
						if(u==n-1) break;
						u=mul(u,u,n);
					}
					if(j==t) return false;
				}
			}
			return true;
		}
		ll gcd(ll a,ll b) {
			return !b?a:gcd(b,a%b);
		}
		ll pollard_rho(ll n) {
			const ll c = randint(1,n-1);
			const auto f = [&](ll x) -> ll {
				return (mul(x,x,n)+c)%n;
			};
			ll s=0,t=0,v=1;
			for(int j=1; ; j<<=1,t=s,v=1) {
				for(int i=1; i<=j; ++i) {
					s=f(s);
					v=mul(v,abs(s-t),n);
					if(!v) return n;
					if(i%127==0) {
						ll d=gcd(v,n);
						if(d>1) return d;
					}
				}
				ll d=gcd(v,n);
				if(d>1) return d;
			}
			return n;
		}
		void split(ll x) {
			if(x<=maxf||x<2||miller_rabin(x)) {
				maxf=max(maxf,x);
				return ;
			}
			ll d=x;
			while(d==x) d=pollard_rho(x);
			while(x%d==0) x/=d;
			split(x);
			split(d);
		}
		int main() {
			scanf("%d",&T);
			while(T--) {
				scanf("%lld",&n);
				maxf=0;
				split(n);
				if(maxf==n) printf("Prime\n");
				else printf("%lld\n",maxf);
			}
			return 0;
		}
		```

[^1]: 请读者自行阅读其他材料
