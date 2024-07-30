---
authors:
  - lnw143
categories:
  - Number Theory
date: 2024-07-09
---

# Miller-Rabin 素性测试

??? note "引理1： 费马小定理"

	$\forall p \in \mathbb P, 0 \le a \lt p, a^{p-1} \equiv 1 \pmod{p}$

	证明略。

??? note "引理2： 二次探测定理"

	$\forall p \in \mathbb P, 0 \le x \lt p$，方程 $x ^ 2 \equiv 1 \pmod p$ 的解为 $x \equiv \pm 1 \pmod p$。
	
	证明略。

设我们要检测的数为 $n > 2$。显然 $n$ 为奇数，设 $n - 1 = 2^s t$，其中 $t$ 为奇数。

若 $n$ 为素数，我们希望 $\forall a \in [1,n)$，满足以下任意条件：

- $a^t \equiv 1 \pmod n$。

- $\exists r \in [0,s), a ^ {2 ^ r t} \equiv -1 \pmod n$。

??? note "证明"

	若以上两项都不满足，显然有 $a^t \not \equiv 1 \pmod n$，这时分两类讨论：

	- $a ^ {n-1} \not \equiv 1 \pmod n$，根据费马小定理，显然 $n$ 不是素数。

	- $a ^ {n-1} \equiv 1 \pmod n$，又因为 $\forall i \in [0,s), a ^ {2 ^ i t} \not \equiv -1 \pmod n$，显然有 $\exists i \in [0,s), a ^ {2 ^ i t} \not \equiv -1 \pmod n$ 且 $a ^ {2 ^ {i + 1} t} \equiv 1 \pmod n$。
	即 $\exists x \in [2,n-1), x ^ 2 \equiv 1 \pmod n$，根据二次探测定理，$n$ 不是素数。

当 $n$ 为合数，若 $a$ 仍满足上述两项之一，则称 $a$ 为 $n$ 的强伪证；若 $a$ 不满足上述两项，则称 $a$ 为 $n$ 不是素数的凭证。

每个奇合数都有许多凭证，但是要找到他们不容易。于是考虑多随机几个 $a$，以验证 $n$ 的素性。

??? note "code"
	=== "C++"
		```cpp
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
		```