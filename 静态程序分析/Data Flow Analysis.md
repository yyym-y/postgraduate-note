
实际上是在分析 Data 是如何在 CFG 上流动的


* **`may analysis`**: 当程序执行路径中存在一条路径可以达到状态 $\displaystyle{  A  }$ ， 那么就认为 $\displaystyle{  A  }$ 会发生，即 over-approximation。 可以确定不漏报， 但是爆出的错误又可能不会发生
* **`must analysis`**： 可以理解为要证据充分，无论从哪一条路径出发，都可以到达状态 $\displaystyle{  A  }$ ， 那么才会认为状态 $\displaystyle{  A  }$ 会发生，可能会漏报，但是只要爆出来，那么这个错误就一定会发生 (under opproximation)
一般情况下，may会初始化为空，must 会初始化为 all， 因为大部分情况下may会取并集，must会取交集
![[Pasted image 20260412160517.png|697]]



通常情况下 `IR` 块值得是一个 `BB` 块， 而一个 IR 块有入口和出口两个部分
不同的程序逻辑可以拆解为三个基本逻辑：顺序执行， 拆分， 汇聚
三种情况的输入输出情况变化如下：
![[Pasted image 20260412161141.png]]
> `IN[s1]` 和 `OUT[s1]` 实际上记录着我们对整个程序的抽象信息，比如关于某个变量是否为正值或者负值等等相关的抽象信息，他们会在不同的IR发生改变

在一个 IR 中，有许多语句 $\displaystyle{  s_{1} ,s_{2}  \dots s_{n} }$ ，那么 `OUT[s1] = f(IN[s1])` ， 其中 $\displaystyle{  f = s_{1} \circ s_{2} \dots \circ s_{n}  }$
> 也可以理解为 `IN[s_{i + 1}] = OUT[s_{i}]`
> 我们只考虑在不同 BB 之间的抽象数据的传递，因为一条一条语句执行会十分消耗时间


抽象数据在不同的 IR 中也可以进行传递，也可以分为三个情况
* 顺序执行 ： `IN[S2] = OUT[S1]`
* 汇聚操作 ：`INT[S_n] = ` $\displaystyle{  \vee  }$ `(OUT[S1], OUT[S2] , ... )` , 其中 $\displaystyle{  \vee  }$ 指某种特定操作
* 分发操作 ： `IN[S2] = IN[S3] = IN[S4] = OUT[S1]`
![[Pasted image 20260412162054.png]]


**到达定值分析** ：
- 假定 x 有定值 d (**definition**)，如果存在一个路径，从紧随 d 的点到达某点 p，并且此路径上面没有 x 的其他定值点，则称 x 的定值 d 到达 (**reaching**) p。
- 如果在这条路径上有对 x 的其它定值，我们说变量 x 的这个定值 d 被杀死 (**killed**) 了
- 这种分析方式可以用作未定义的检测

对所有的定义语句，我们可以用一个 `bitset` 来表示：
![[image.avif]]

这是一个迭代算法，其迭代算法的伪代码为：
![[image 1.avif]]

有两个核心的语句：
*  `IN[B] = U_{p in B predecessor} OUT[P]` : 汇聚所有的输入（取并集）
* `OUT[B] = gen_b U (IN[B] - kill_B)` : 其 原理是加入这个IR中的定义，之后清楚原本的定义

这个算法一定会收敛，因为 bitset 中的 1 永远不可能变成 0， 同时输入一定则输出一定

![[Pasted image 20260412163421.png|673]]

其中的结果的含义： ($\displaystyle{  \text{001010000}  }$) , 如果 $\displaystyle{  i  }$ 下标为 $\displaystyle{  1  }$ ， 那么 $\displaystyle{  D_{i}  }$ 这个定义可以影响到这个IR

