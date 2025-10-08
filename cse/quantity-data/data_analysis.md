+ [资料分析](#资料分析)
  + [基础公式](#基础公式)
  + [比较与计算](#比较与计算)
    + [基期量](#基期量)
    + [增长量](#增长量)
    + [增长率](#增长率)
  + [特殊增长率](#特殊增长率)
    + [名义/实际增长率](#名义实际增长率)
    + [拉动增长率](#拉动增长率)
    + [间隔增长率](#间隔增长率)
    + [年均增长率](#年均增长率)
  + [比重（均值）](#比重均值)
    + [基期比重（均值）](#基期比重均值)
    + [两期比重（均值）](#两期比重均值)
    + [均值增长率](#均值增长率)
  + [速算](#速算)
    + [百化分](#百化分)
    + [平方数](#平方数)
    + [高次幂](#高次幂)
    + [分数比较](#分数比较)
  + [综合分析策略](#综合分析策略)

# 资料分析

## 基础公式

$$
\begin{aligned}
B &= \frac{C}{1+r} \\
\Delta &= \frac{C}{1+r} \cdot r \\
r &= \frac{\Delta}{B} = \frac{\Delta}{C - \Delta}
\end{aligned}
$$

## 比较与计算

### 基期量

> [!TIP]
> 当 $|r| \leq 5\%$ 时， $\displaystyle \frac{C}{1+r} = \frac{C(1-r)}{(1+r)(1-r)} \approx C(1-r)$
> 
> 适用范围**不**包括：选项首位相同，第二位也相同，**且** $4\% \leq |r| \leq 5\%$

### 增长量

> [!TIP]
> $$
> \left.
> \Delta = 
> \begin{cases}
> \displaystyle\frac{C}{\rho+1}, & r > 0 \\
> \displaystyle-\frac{C}{\rho-1}, & r < 0
> \end{cases}
> \right\} \quad \text{if } |r| = \frac{1}{\rho}
> $$

### 增长率

> [!TIP]
> $\displaystyle \frac{\Delta}{C}$ 越大， $r$ 越大
> 
> $$
> r = \frac{1}{\frac{C}{\Delta}-1}
> $$

## 特殊增长率

### 名义/实际增长率

$$
1 + r_n = (1 + r_r)(1 + \pi)
$$

其中：

+ $r_n$ ：名义增长率
+ $r_r$ ：实际增长率
+ $\pi$ ：通货膨胀率

### 拉动增长率

$$
r_{\mathrm{pull}} = \frac{\Delta}{B}
$$

### 间隔增长率

$$
r_i = \frac{C}{B} -1 = \frac{B \cdot (1+r_1)(1+r_2)}{B} -1 = \boxed{ r_1 + r_2 + r_1 \cdot r_2 }
$$

+ $r_1, r_2$ ：相邻增长率

### 年均增长率

$$
r_{\mathrm{avg}} = \sqrt[n]{\frac{C}{B}} - 1
$$

## 比重（均值）

### 基期比重（均值）

$$
\frac{A}{1+a} \div \frac{B}{1+b} = \boxed{ \frac{A}{B} \cdot \frac{1+b}{1+a} }
$$

### 两期比重（均值）

$$
\frac{A}{B} - \frac{A}{B} \cdot \frac{1+b}{1+a} = \boxed{ \frac{A}{B} \cdot \frac{a-b}{1+a} }
$$

### 均值增长率

$$
\boxed{ r_\mathrm{avg} = \frac{a-b}{1+b} }
$$

## 速算

### 百化分

![百化分](images/percent2frac.png)

| 百分数 |             分数             | 百分数 |             分数             |
| :----: | :--------------------------: | :----: | :--------------------------: |
| 16.7%  | $\displaystyle \frac{1}{6}$  |  5.6%  | $\displaystyle \frac{1}{18}$ |
| 14.3%  | $\displaystyle \frac{1}{7}$  |  5.3%  | $\displaystyle \frac{1}{19}$ |
| 12.5%  | $\displaystyle \frac{1}{8}$  |  4.8%  | $\displaystyle \frac{1}{21}$ |
| 11.1%  | $\displaystyle \frac{1}{9}$  |  4.5%  | $\displaystyle \frac{1}{22}$ |
|  9.1%  | $\displaystyle \frac{1}{11}$ |  4.3%  | $\displaystyle \frac{1}{23}$ |
|  8.3%  | $\displaystyle \frac{1}{12}$ |  4.2%  | $\displaystyle \frac{1}{24}$ |
|  7.7%  | $\displaystyle \frac{1}{13}$ |  3.8%  | $\displaystyle \frac{1}{26}$ |
|  7.1%  | $\displaystyle \frac{1}{14}$ |  3.7%  | $\displaystyle \frac{1}{27}$ |
|  6.7%  | $\displaystyle \frac{1}{15}$ |  3.6%  | $\displaystyle \frac{1}{28}$ |
| 6.25%  | $\displaystyle \frac{1}{16}$ |  3.4%  | $\displaystyle \frac{1}{29}$ |
|  5.9%  | $\displaystyle \frac{1}{17}$ |  3.3%  | $\displaystyle \frac{1}{30}$ |

### 平方数

|  $x$  | $x^2$ |
| :---: | :---: |
|  17   |  289  |
|  18   |  324  |
|  19   |  361  |

|  $x$  | $x^2$ |  $x$  | $x^2$ |
| :---: | :---: | :---: | :---: |
|  21   |  441  |  29   |  841  |
|  22   |  484  |  28   |  784  |
|  23   |  529  |  27   |  729  |
|  24   |  576  |  26   |  676  |

### 高次幂

|  $x$  | $x^3$ |
| :---: | :---: |
|  1.1  |  1.3  |
|  1.2  |  1.7  |
|  1.3  |  2.2  |
|  1.4  |  2.7  |

|  $x$  | $x^4$ |
| :---: | :---: |
|  1.1  |  1.5  |
|  1.2  |  2.1  |
|  1.3  |  2.9  |
|  1.4  |  3.8  |

### 分数比较

> [!TIP]
> 分子快，分子大的分数大
> 
> 分母快，分母大的分数小

## 综合分析策略

> [!TIP]
> 1. **分析性选项**：两期比例比较、混合增长率、当月与前几月均值比较
> 2. **表述**
>    + 易错：“均”“都”“同一个”“逐年上升/下降”“首超”
>    + 易对：“约”
> 3. **陷阱**
>    + “多几倍”
>    + 日均、月均（忘记除以天数）
>    + 单位
> 4. 由易到难：找数->加减->一步除法->增长量->增长率->基期/两期比例计算