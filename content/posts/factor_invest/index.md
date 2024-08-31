+++
title = '因子投资慢读计划'
date = 2024-08-28T00:25:16+08:00
tags = ['因子', '自学']
description = '每天读一点，不着急'
draft = false
math = true
+++

原书pdf: 《[因子投资 方法与实践](因子投资_方法与实践.pdf)》

# 回归检验

## 时间序列回归

对某只股票 `$i$`

`$$
R_{it}^e=\alpha_i+\beta_i^T\lambda_t+\epsilon_t, \quad t=1,2,\cdots,T
$$`

时序回归：首先固定`$i$`，以这只股票在不同时间上的数据为样本，做[OLS回归](/blog/chapters/OLS回归)，得到属于这只股票的`$\beta_i^T,\alpha_i,\epsilon_i$`。

![时序回归](时序回归.png)

这是一张散点图，横轴是用时序回归估计出来的 `$\hat{\beta}_i$` ，纵轴是通过公式 `$E_t[R_{it}^e]=\hat{\alpha}_i+\hat{\beta}_iE_t[\lambda_t]$` 估计出来的预期收益率。因此，这张图不是最小化 `$\alpha_i$` 的结果。

既然我们已经估计出 `$\alpha_i$`，下面要检验其是否联合为零。常用的方法是[GRS检验](/blog/chapters/grs)。


