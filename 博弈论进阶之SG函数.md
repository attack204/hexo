---
title: 博弈论进阶之SG函数
date: 2018-02-23 14:32:47
tags:
- oi
- 博弈论
- SG函数
categories:
- oi
---

<Excerpt in index >
## SG函数

个人理解：SG函数是人们在研究博弈论的道路上迈出的重要一步，它把许多杂乱无章的博弈游戏通过某种规则结合在了一起，使得一类普遍的博弈问题得到了解决。

从SG函数开始，我们不再是单纯的同过找规律等方法去解决博弈问题，而是需要学习一些博弈论中基本的定理，来找到他们的共同特点
<!-- more -->

<The rest of contents >


那么就先介绍几个最基本的定理(也可以叫常识)吧

## 基本定理

### ICG游戏
1.游戏有两个人参与，二者轮流做出决策。且这两个人的决策都对自己最有利。

2.当有一人无法做出决策时游戏结束，无法做出决策的人输。无论二者如何做出决策，游戏可以在有限步内结束。

3.游戏中的同一个状态不可能多次抵达。且游戏不会有平局出现。任意一个游戏者在某一确定状态可以作出的决策集合只与当前的状态有关，而与游戏者无关。

满足上述条件的问题我们称之为ICG游戏，ICG游戏属于组合游戏

最典型的[nim游戏](http://attack204.com/2018/02/22/%E5%8D%9A%E5%BC%88%E8%AE%BA%E5%85%A5%E9%97%A8%E4%B9%8Bnim%E6%B8%B8%E6%88%8F/)，就是一种ICG游戏

### 必胜态与必败态


定义P-position与N-position

P-position：必败态(简记为`P`)，即Previous-position,你可以直观的认为处于这种状态的人一定会输

N-position：必胜态(简记为`N`)，即Next-position，你可以直观的理解为处于这种状态的人一定会赢

这仅仅是最直观的定义

更严谨的定义为：

1. 无法移动的状态(即terminal-position)为P
2. `可以`移动到P的局面为N
3. `所有`移动都会进入N的局面为P


### DAG(有向无环图)中的博弈

在正式研究$SG$函数之前，我们先来研究一下DAG中的博弈

>给定一张有向无环图，在起始定点有一枚棋子，两个顶尖聪明的人交替移动这枚棋子，不能移动的人算输

不要小看这个游戏，事实上，所有ICG问题都可以抽象为这种游戏(即把初始局面看做顶点，把从一个状态可以到另一个状态之间连边)

## SG函数

下面我们来正式研究一下SG(Sprague-Grundy)函数

首先定义`mex`运算，这是一种集合中的运算，它表示`最小的不属于集合的非负整数`

例如$mex\{1,2,3\}=0$，$mex\{0,2\}=1$，$mex\{0,1,2,3\}=4$，$mex\{\}=0$


对于给定的有向无环图，定义每个点的SG函数为

$SG(x)=mex \{\  SG(y)\ |\ x \  can\ go\ to\ y \}$


然而单单一个这样的空洞的函数是解决不了问题的，我们需要分析一下它的性质

- 所有汇点的$SG$函数为$0$

这个性质比较显然，因为汇点的所有后继状态都是空集

- 当$SG(x)=0$时,该节点为必败点

由$SG$函数的性质易知该节点的所有后继节点$SG$值均不为$0$

满足必败态的定义

- 当$SG(x)\neq 0 $,该节点为必胜点

由$SG$函数的定义可知该节点的后继节点中一定有一个节点$SG=0$

满足必胜态的定义


这样我们通过最基本的$SG$值的定义，我们就可以判断出一个状态是必胜态还是必败态


这个问题实际上就是我们前面讲的[巴什博奕](http://attack204.com/2018/02/22/%E5%8D%9A%E5%BC%88%E8%AE%BA%E5%85%A5%E9%97%A8%E4%B9%8B%E5%B7%B4%E4%BB%80%E5%8D%9A%E5%A5%95/)

如果这个问题再复杂一点呢？

当这个棋盘上有$n$个棋子的时候呢？

其实它们的分析思路是一样的

当$SG(x)=k$时，它表明后继状态中含有$SG(y)=1 \dots k-1$

也就是说，我们从$k$可以转移到$1 \dots k-1$中的任何一个状态，而当前共有$n$个棋子。

这会让你想到什么？

nim取石子游戏！

那我们是不是也可以推出：

如果在nim游戏中的$n$堆石子的$SG$值异或和不为$0$就说明先手必胜呢？

这是肯定的，因为当你打出nim游戏的$SG$值表时就会发现，$SG_{nim}(x)=x$

是不是很神奇？


## SG定理

SG函数的应用远远不止和巴什博奕与nim游戏有关，我们回过头来考虑能否把SG函数推广开来

类比nim取石子游戏的思路，我们可不可以大胆设想：

**`游戏的和的SG值是他们的SG值的xor`**

暂且不管这个结论对不对，我们设想一下，假如这个结论对的话，会有什么后果.

我们可以将ICG问题对应到DAG上，然后直接通过SG函数之间的转移而解决几乎全部的问题

是不是很令人兴奋？

更令人兴奋的是，这个定理是正确的！

什么？证明？

如果你是一个追求完美的人可以看[这里](https://www.zhihu.com/question/51290443/answer/125105697)

如果你像我一样连线性代数都不知道是什么的话大概就是从DAG上归纳一下就好了吧

## SG定理的应用

SG定理的应用非常的广泛，几乎所有的博弈类问题都有它的影子，本文仅仅是简单的介绍一下这个定理，更深层次的应用以后会补充的

上面提到了SG函数，那么SG函数的值是怎么计算的呢？

很简单，我们直接通过$mex$运算的定义就可以计算了

```cpp
int F[MAXN];//可以转移的状态集合，一般题目会给出 
int S[MAXN];//表示该点可以转移到的状态有哪些 
int SG[MAXN];//该点的SG值 
void GetSG()
{
    for(int i=1;i<=N;i++)//枚举DAG中所有点 
    {
        memset(S,0,sizeof(S));//初始化
        for(int j=1;j<=limit&&F[j]<=i;j++)//limit表示转移的集合的大小
            S[SG[i-F[j]]]=1; 
        for(int j=1;;j++)
            if(!S[j])
                {SG[i]=i;break;}//根据定义计算SG函数 
    }
}
```

来一道[裸题](http://acm.hdu.edu.cn/showproblem.php?pid=1848)

**[题解](http://www.cnblogs.com/zwfymqz/p/8459272.html)**

