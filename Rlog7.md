# What did you learn？

> Attacks

## [Number 33: How does the Bellcore attack work against RSA with CRT?](https://bristolcrypto.blogspot.com/2015/05/52-things-number-33-how-does-bellcore.html)

> 钟核攻击？🐷

RSA-CRT (#21) 是一种加速 RSA 加密和解密的方法，但是如果实现 RSA 的硬件（或软件）不时地产生错误或故障，那么就可以利用其来进行攻击。也就是我们接下来要介绍的。

RSA 是基于大整数分解问题的公钥密码算法，对其攻击往往涉及对模数的因式分解，但是 [Boneh、DeMillo 和 Lipton](https://link.springer.com/article/10.1007/s001450010016) 找到一种攻击，可以避免直接分解模数。他们表明，错误的密码值可以被攻击者利用，进而危害安全性。

首先回忆一下 RSA-CRT ：

本来是是计算 $S=x^d\ mod\ N$，现在首先计算 $S_1=x^{d}\ mod\ p$ 和 $S2=x^{d}\ mod\ q$，然后再通过 CRT 计算 $S$。

如果我们需要对同一消息 $x$ 的进行两次签名, 一次签名正确 $S=x^d\ mod\ N$，另一个签名 $\hat{S}$ 将错误。
首先需要计算 $S_1$ 和 $S2$，同样的，也需要计算 $\hat{S_1}$ 和 $\hat{S2}$。
假设仅在计算 $\hat{S_1}$ 时出现了错误，就会造成 $S_1 \neq \hat{S_1}\ mod\ p$，但是 $S_2 = \hat{S_2}$。也就意味着 $S \neq \hat{S}\ mod\ p,S = \hat{S}\ mod\ q$。也就有了：
$$
gcd(S-\hat{S},N) = q
$$
结果就是，仅用一个错误签名就成功分解了 $N$ 。

## [Number 34: Describe the Baby-Step/Giant-Step method for breaking DLPs](https://bristolcrypto.blogspot.com/2015/05/52-things-number-15-describe-baby.html)

[Baby-Step/Giant-Step](https://en.wikipedia.org/wiki/Baby-step_giant-step) 是由 [Daniel Shanks](https://en.wikipedia.org/wiki/Daniel_Shanks) 提出的一种方法，用于解决离散对数问题（Discrete Logarithm Problem，DLP）。

### DLP

给定一个 阶为 $n$ 循环群 $G$，一个生成元 $g$ 和一个元素 $h$，DLP 就是为了找到一个整数 $x$，使得 $g^x = h$。这个问题是一个困难问题。

### Baby-Step/Giant-Step

因为 $n$ 是群的 order，所以对于一个 $0\leq x\leq n$，我们可以将 $x$ 写为：
$$
x = i\lceil{\sqrt{n}}\rceil + j \tag{1}
$$
其中 $0\leq i,j\leq \lceil{\sqrt{n}}\rceil$。

因此 DLP 可以转化为：
$$
h=g^{i\lceil{\sqrt{n}}\rceil +j}\\
h(g^{-j})=g^{i\lceil{\sqrt{n}}\rceil}
$$
现在问题就找到一个满足等式的对儿 $(i,j)$。

一个方式就是预计算一张表 $\{g^{i\lceil{\sqrt{n}}\rceil}\},0\leq i \leq \sqrt{n}$ 与 $g^{-1}$
然后就是通过迭代 $j$ 计算 $h(g^{-1})^j$，直到找到一个匹配的值。
然后就可以通过 $(1)$ 计算出 $x$。

此方法的时间和空间复杂度均为 $O(\sqrt{n})$，但是目前还构不成威胁。

## [Number 35: Give the rough idea of Pollard rho, Pollard "kangaroo" and parallel Pollard rho attacks on ECDLP.](https://bristolcrypto.blogspot.com/2015/06/52-things-number-35-give-rough-idea-of.html)

本章将讨论空间开销更小的算法，但是复杂度还是 $O(\sqrt{n})$。





