+ [资料分析](#资料分析)
  + [基期与现期](#基期与现期)
  + [增长量](#增长量)
    + [年均增长量](#年均增长量)
  + [增长率](#增长率)
    + [名义增长率与实际增长率](#名义增长率与实际增长率)
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

# 资料分析

## 基期与现期

$$
B = \frac{C}{1+r}
$$

其中：

+ $B(Base)$ ：基期量
+ $C(Current)$ ：现期量
+ $r(rate)$ ：增长率

> [!TIP]
> 当 $|r| \leq 5\%$ 时， $\displaystyle \frac{C}{1+r} = \frac{C(1-r)}{(1+r)(1-r)} \approx C(1-r)$
> 
> 适用范围**不**包括：选项首位相同，第二位也相同，**且** $4\% \leq |r| \leq 5\%$

## 增长量

$$
\Delta = B \cdot r = \frac{C}{1+r} \cdot r
$$

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



### 年均增长量

$$
\Delta_{\mathrm{avg}} = C \cdot \frac{r}{n(1+r)}
$$

其中：

+ $n$ ：时间跨度（年数）

## 增长率

### 名义增长率与实际增长率

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
r_i = \frac{C}{B} -1 = \frac{B \cdot (1+r_1)(1+r_2)}{B} -1 = r_1 + r_2 + r_1 \cdot r_2
$$

其中：

+ $r_1, r_2$ ：相邻增长率

### 年均增长率

$$
r_{\mathrm{avg}} = \sqrt[n]{\frac{C}{B}} - 1
$$

## 比重（均值）

### 基期比重（均值）

$$
\frac{A}{1+a} \div \frac{B}{1+b} = \frac{A}{B} \cdot \frac{1+b}{1+a}
$$

其中：

+ $A$ ：现期部分量
+ $B$ ：现期整体量
+ $a$ ：部分量的增长率
+ $b$ ：整体量的增长率

### 两期比重（均值）

$$
\frac{A}{B} - \frac{A}{B} \cdot \frac{1+b}{1+a} = \frac{A}{B} \cdot \frac{a-b}{1+a}
$$

### 均值增长率

$$
r_\mathrm{avg} = \frac{a-b}{1+b}
$$

## 速算

### 百化分

![百化分](images/percent2frac.png)

| 百分数 |             分数              |
| -----: | :---------------------------: |
|  18.2% | $\displaystyle\frac{1}{5.5}$  |
|  16.7% |  $\displaystyle\frac{1}{6}$   |
|  15.4% | $\displaystyle\frac{1}{6.5}$  |
|  14.3% |  $\displaystyle\frac{1}{7}$   |
|  13.3% | $\displaystyle\frac{1}{7.5}$  |
|  12.5% |  $\displaystyle\frac{1}{8}$   |
|  11.8% | $\displaystyle\frac{1}{8.5}$  |
|  11.1% |  $\displaystyle\frac{1}{9}$   |
|  10.5% | $\displaystyle\frac{1}{9.5}$  |
|   9.1% |  $\displaystyle\frac{1}{11}$  |
|   8.7% | $\displaystyle\frac{1}{11.5}$ |
|   8.3% |  $\displaystyle\frac{1}{12}$  |
|     8% | $\displaystyle\frac{1}{12.5}$ |
|   7.7% |  $\displaystyle\frac{1}{13}$  |
|   7.4% | $\displaystyle\frac{1}{13.5}$ |
|   7.1% |  $\displaystyle\frac{1}{14}$  |
|   6.9% | $\displaystyle\frac{1}{14.5}$ |
|   6.7% |  $\displaystyle\frac{1}{15}$  |
|   6.5% | $\displaystyle\frac{1}{15.5}$ |
|  6.25% |  $\displaystyle\frac{1}{16}$  |
|   6.1% | $\displaystyle\frac{1}{16.5}$ |
|   5.9% |  $\displaystyle\frac{1}{17}$  |
|   5.6% |  $\displaystyle\frac{1}{18}$  |
|   5.3% |  $\displaystyle\frac{1}{19}$  |

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