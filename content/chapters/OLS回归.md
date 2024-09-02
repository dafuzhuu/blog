+++
title = 'OLS回归'
date = 2024-08-31T12:09:29+08:00
draft = false
math = true
hidden = true
+++

# OLS的假设

1. **线性（Linearity）** 
   回归模型假定因变量 `\( Y \)` 和自变量 `\( X \)` 之间的关系是线性的，即模型可以表示为：
   `\[
   Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \ldots + \beta_n X_n + \epsilon
   \]`
   其中 \( \epsilon \) 是误差项。

2. **独立性（Independence）：** 
   假定所有观测值的误差项 `\( \epsilon \)` 是相互独立的，即 `\(\epsilon_i\)` 与 `\(\epsilon_j\)`（当 `\( i \neq j \)` 时）是独立的。

3. **同方差（Homoscedasticity）：** 
   假定误差项的方差是常数，即所有观测值的误差项 `\( \epsilon_i \)` 具有相同的方差：
   `\[
   \text{Var}(\epsilon_i) = \sigma^2
   \]`
   对所有 `\( i \)`。

4. **服从正态分布（Normality）：** 
   假定误差项 `\( \epsilon \)` 服从正态分布，即：
   `\[
   \epsilon \sim N(0, \sigma^2)
   \]`
   该假设主要用于推断检验，例如 t 检验和 F 检验。

5. **无多重共线性（No Multicollinearity）：** 
   假定自变量之间没有完全的线性关系，即自变量矩阵 `\( X \)` 的列向量是线性独立的。

6. **误差项的期望为零（Zero Mean）：**
   假定误差项的期望值为零，即：
   `\[
   \mathbb{E}[\epsilon] = 0
   \]`


# OLS推导
OLS回归的步骤如下

`$$
\mathbf{R}_i = \mathbf{X}_i \mathbf{b}_i + \boldsymbol{\epsilon}_i
$$`

其中

`$$\mathbf{R}_i = \begin{pmatrix}
R_{i1}^e \\
R_{i2}^e \\
\vdots \\
R_{iT}^e\\
\end{pmatrix}_{T\times 1},
\mathbf{X}_i = \begin{pmatrix}
1 & \lambda_1^T \\
1 & \lambda_2^T \\
\vdots & \vdots \\
1 & \lambda_T^T
\end{pmatrix}_{T \times (k+1)},
\mathbf{b}_i = \begin{pmatrix}
\alpha_i \\
\beta_i
\end{pmatrix}_{(k+1) \times 1}, 
\boldsymbol{\epsilon}_i = \begin{pmatrix}
\epsilon_{i1} \\
\epsilon_{i2} \\
\vdots \\
\epsilon_{iT}
\end{pmatrix}_{T\times 1}
$$`

我们希望通过最小化误差平方和来估计 `$\mathbf{b}_i$`，即最小化以下目标函数：

`$$
S(\mathbf{b}_i) = \sum_{t=1}^T \epsilon_{it}^2 = \boldsymbol{\epsilon}_i^T \boldsymbol{\epsilon}_i
$$`

其中 `$\boldsymbol{\epsilon}_i = \mathbf{R}_i - \mathbf{X}_i \mathbf{b}_i$`，所以我们可以将目标函数写为：

`$$
S(\mathbf{b}_i) = (\mathbf{R}_i - \mathbf{X}_i \mathbf{b}_i)^2 = (\mathbf{R}_i - \mathbf{X}_i \mathbf{b}_i)^T (\mathbf{R}_i - \mathbf{X}_i \mathbf{b}_i)
$$`

展开后得到：

`$$
S(\mathbf{b}_i) = \mathbf{R}_i^T \mathbf{R}_i - 2\mathbf{b}_i^T \mathbf{X}_i^T \mathbf{R}_i + \mathbf{b}_i^T \mathbf{X}_i^T \mathbf{X}_i \mathbf{b}_i
$$`

为了找到最小值，我们需要对 `$S(\mathbf{b}_i)$` 关于 `$\mathbf{b}_i$` 求导并令其等于零：

`$$
\frac{\partial S(\mathbf{b}_i)}{\partial \mathbf{b}_i} = -2\mathbf{X}_i^T \mathbf{R}_i + 2\mathbf{X}_i^T \mathbf{X}_i \mathbf{b}_i = 0
$$`

简化后得到：

`$$
\mathbf{X}_i^T \mathbf{X}_i \mathbf{b}_i = \mathbf{X}_i^T \mathbf{R}_i
$$`

由于 `$\mathbf{X}_i^T \mathbf{X}_i$` 是可逆的（假设不存在多重共线性），我们可以得到：

`$$
\hat{\mathbf{b}}_i = (\mathbf{X}_i^T \mathbf{X}_i)^{-1} \mathbf{X}_i^T \mathbf{R}_i
$$`

`$\hat{\mathbf{b}}_i$` 是回归系数的估计值，包括 `$\hat{\alpha}_i$` 和 `$\hat{\beta}_i$`：

`$$
\hat{\mathbf{b}}_i = \begin{pmatrix}
\hat{\alpha}_i \\
\hat{\beta}_i
\end{pmatrix}
$$`

误差项 `$\epsilon_{it}$` 的估计值可以通过以下公式得到：

`$$
\hat{\boldsymbol{\epsilon}}_i = \mathbf{R}_i - \mathbf{X}_i \hat{\mathbf{b}}_i
$$`