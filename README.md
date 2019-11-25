# Poisson Distribution

### Binomial Distribution

讲Poisson Distribution之前，我们先聊聊Binomial Distribution，如果你已经熟悉这部分，可以略过。

场景：假设我们扔一枚硬币（无论是否均匀），得到head的概率P\(H\) = 0.7，得到tail的概率P\(T\) = 0.3。

假设我们投该硬币n次，

* 那么得到head的次数的期望E\(X\) = n · P\(H\)
* 假设 p = P\(H\) = 0.7，则得到k次head的概率为：

$$
P(X=k) = C_n^k \cdot p^k\cdot(1-p)^{n-k}
$$

#### 关于二项分布的期望为什么是 E\(X\) = n · p  的证明：

$$
E(X) = \sum_{k=0}^n k\cdot C_n^k\cdot p^k \cdot(1-p)^{n-k}\\
= 0 \cdot C_n^0 \cdot p^0 \cdot (1-p)^n + 1 \cdot C_n^1 \cdot p^1 \cdot (1-p)^{n-1} + ...  + n \cdot C_n^n \cdot p^n \cdot (1-p)^0 \\
= 0 + 1 \cdot C_n^1 \cdot p^1 \cdot (1-p)^{n-1} + ...  + n \cdot C_n^n \cdot p^n \cdot (1-p)^0 \\
= \sum_{k=1}^n k \cdot C_n^k \cdot p^k \cdot (1-p)^{n-k} \\
= \sum_{k=1}^n k \cdot \frac{n!}{k! \cdot (n-k)!} \cdot p^k \cdot (1-p)^{n-k} \\
= \sum_{k=1}^n \frac{n!}{(k-1)! \cdot (n-k)!} \cdot p^k \cdot (1-p)^{n-k} \\
= \sum_{k=1}^n \frac{n \cdot (n-1)!}{(k-1)! \cdot(n-k)!} \cdot p \cdot p^{k-1} \cdot (1-p)^{n-k} \\ 
= np \sum_{k=1}^{n} \frac{(n-1)!}{(k-1)!(n-k)!} \cdot p^{k-1} \cdot (1-p)^{n-k} \\
\text{接下来我们令：} a =k-1, b=n-1 \\
\text{则有，}a+1=k, b+1=n, n-k=b-a \\
\text{我们用}a, b\text{来替换掉上面的}k, n \text{则上式等于：} \\
= np\sum_{a=0}^{b} \frac{b!}{a! \cdot (b-a)!} \cdot p^a \cdot (1-p)^{b-a}\\
= np\sum_{a=0}^{b} C_b^a \cdot p^a \cdot (1-p)^{b-a}
\text{上式求和部分正好等于1，因为它的实际意义是：}\\
\text{进行}b\text{次相同的扔硬币实验，对所有可能出现的head的次数的求和（从0次到b次），}\\
\text{显然求和项为1。从而上式就等于：} np
$$

### Poisson Distribution

终于进入正题了，泊松分布在交通领域有比较广泛的应用，尤其是在“一段时间内通过道路某一断面的车辆数”上非常有用。

我们定义一个变量X，表示一小时内通过某个位置的车辆数。

此时我们需要两个假设：

1. 每个小时之间，通过该位置的车辆数基本相同（为了避免早高峰之类的情况）；
2. 在当前小时通过一定数量的车辆后，不意味着下一个小时的车辆数会变多或变少（为了保证每个小时通过的车辆数之间是相互独立的）。

#### 案例建模

如果我们是traffic engineer，我们可以利用上面提到过的二项分布来尝试对该问题进行建模。

假设每一分钟内最多有1辆车会通过该位置，那么1小时内，相当于我们做了60次实验，该实验的结果服从二项分布。当然，一分钟可能会有点大，我们可以缩小为 每秒，每毫秒，甚至更小。

那么，根据二项分布，我们有E\(X\) = np = λ  \(unit: vehicles/hour\)。其中 ：

* λ 是每小时通过该位置的车辆数
* n 为60，也就是在一小时内，以分钟为间隔，我们做了60次独立实验

那么 p = 60/λ

进而，每小时内有k辆车通过该位置的概率P\(X=k\)为：

$$
P(X=k) = C_{60}^k \cdot (\frac{λ}{60})^k \cdot (1-\frac{λ}{60})^{60-k} \tag{1.1}
$$

好的，现在我们有了一个比较简单的模型了。就像上面提到的，如果一分钟内有不止一辆车通过该位置怎么办？——切分成秒级。同理如果一秒内也有不止一辆车通过该位置怎么办？——切分成毫秒级。

那我们就需要无限切分，也就是Δt→0的极限（**求出的这个极限值就是Poisson Distribution**）：

* 在我们推导公式之前，我们先引入下面两个等式（如果不了解这两个灯饰等式，推荐看一下先本文最后的注释）。

$$
①\lim_{x\rightarrow‎\infty}(1+\frac{a}{x})^x = e^a \text{,} \space \space a \text{为常数} \\
② \frac{x!}{(x-k)!} = x \cdot (x-1) \cdot (x-2)  \cdot \cdot \cdot  (x-k+1) \\
③\lim_{x\rightarrow a}{f(x) \cdot g(x)} = \lim_{x\rightarrow a}{f(x)} \cdot \lim_{x\rightarrow a}{g(x)}
$$

现在我们来计算上面\(1.1\)式的极限：

$$
P(X=k) = \lim_{n\rightarrow\infty} C_n^k \cdot (\frac{λ}{n})^k \cdot (1-\frac{λ}{n})^{n-k} \\
= \lim_{n\rightarrow\infty}\frac{n!}{(n-k)! \cdot k!} \cdot \frac{λ^k}{n^k} \cdot (1-\frac{λ}{n})^n \cdot (1-\frac{λ}{n})^{-k}\\
= \lim_{n\rightarrow\infty}\frac{n\cdot(n-1)\cdot(n-2)\cdot\cdot\cdot(n-k+1)}{n^k} \cdot \frac{λ^k}{k!} (1-\frac{λ}{n})^n \cdot (1-\frac{λ}{n})^{-k}\\
\text{注意，最前面一项分式的分子是从} n\text{到} (n-k+1)\text{共}k \text{项。}\\
\text{因此，分子展开后的多项式的最高次项指数为}k。 \\
\text{所以第一项分式的极限（根据洛必达法则）就是1} \\
\text{然后我们根据③，将乘法拆开分别求极限，得到如下式子：}\\
= \lim_{x\rightarrow \infty} {\frac{n^k + ...}{n^k}}\cdot \lim_{x\rightarrow \infty}{\frac{λ^k}{k!} \cdot \lim_{x\rightarrow \infty}{(1-\frac{λ}{n})^n \cdot \lim_{x\rightarrow \infty}(1-\frac{λ}{n})^{-k}}} \\
= 1 \cdot \frac{λ^k}{k!} \cdot e^{-λ} \cdot 1 \space \space \text{(注意，第三项是根据上面的①得到的)}\\
= \frac{λ^k}{k!} \cdot e^{-λ}
$$

### 注释

$$
\lim_{x\rightarrow‎\infty}(1+\frac{a}{x})^x = e^a \text{,} \space \space a \text{为常数。证明：}\\
\text{令} \frac{1}{n}=\frac{a}{x}\text{，则有}x=n \cdot a \text{。那么：} \\
\lim_{x\rightarrow‎\infty}(1+\frac{a}{x})^x = \lim_{n\rightarrow‎\infty}(1+\frac{1}{n})^{x \cdot a} = \lim_{n\rightarrow‎\infty}{((1+\frac{1}{n})^n)^a} \\
\text{其中}\lim_{n\rightarrow‎\infty}{(1+\frac{1} {n})^n} \text{这部分是趋向于}e \text{的，所以上面的等式得证。}
$$













