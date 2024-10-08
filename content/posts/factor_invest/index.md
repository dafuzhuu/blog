+++
title = '因子投资慢读计划'
date = 2024-08-28T00:25:16+08:00
tags = ['因子', '自学']
description = '每天读一点，不着急'
draft = true
math = true
+++

原书pdf: 《[因子投资 方法与实践](因子投资_方法与实践.pdf)》

# 前言

在研究了几天的GRS检验却越来越迷糊后，我决定不再纠结于书中命题的证明细节，而是关注如何实现。同时，意识到本科所学的线性代数和统计学实在不够用，我决定先将两者重学一遍，再学更高等的线代和统计，这些完成以后再来研究本书的技术细节。届时我已经把握了这本书的核心概念、以及编程上如何实现，将更有把握完成数学上的理解。

# 投资组合排序法

1. 计算每只股票对应的因子值
2. 按大小排序，等分为10组
3. 做多排名最高的组合，做空排名最低的组合
4. 多空组合的收益就是**因子的预期收益率**

计算多空组合每期的收益率，记为 `$\lambda_t$`，构成一个收益率序列

`$$
[\lambda_1,\lambda_2,\cdots,\lambda_T]
$$`

这就是因子预期收益率（或简称因子收益率）的时序数据。

考虑到股票在不同因子上都有暴露，这样基于一种因子的排序可能参杂了其他因子的影响，因此有双重排序法。双重排序法又分为两种：

- 独立双重排序：将股票池按 `$X_1,X_2$` 分别排序，各分成 `$L_1,L_2$` 组，这就构建了 `$L_1\times L_2$` 个组合。这么做的前提是样本量要非常大才能使得每个小组合内的股票数目足够
- 条件双重排序：先按 `$X_1$` 排序，分成 `$L_1$` 组，再到每组内部按 `$X_2$` 排序。这么做相当于先控制变量(`$X_1$`)，然后考察因子 `$X_2$` 的收益率。可以避免计算
`$X_2$` 的收益率时受到 `$X_1$` 的影响。

# 回归检验

核心公式

$$
E_T[R_{i}^e]=\alpha_i+\beta_i'\lambda
$$

角标 `$T$` 表示在时序上取平均

## 时间序列回归

对某只股票 `$i$`

`$$
R_{it}^e=\alpha_i+\beta_i^T\lambda_t+\epsilon_t, \quad t=1,2,\cdots,T
$$`

时序回归：首先固定`$i$`，以这只股票在不同时间上的数据为样本，做[OLS回归](/blog/chapters/ols回归)，得到属于这只股票的`$\beta_i,\alpha_i,\epsilon_i$`。

![时序回归](时序回归.png)

这是一张散点图，横轴是用时序回归估计出来的 `$\hat{\beta}_i$` ，纵轴是通过公式 `$E_t[R_{it}^e]=\hat{\alpha}_i+\hat{\beta}_iE_t[\lambda_t]$` 估计出来的预期收益率。因此，这张图不是最小化 `$\alpha_i$` 的结果。

既然我们已经估计出 `$\alpha_i$`，下面要检验其是否联合为零。常用的方法是[GRS检验](/blog/chapters/grs)。


