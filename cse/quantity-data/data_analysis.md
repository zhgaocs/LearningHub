+ [资料分析](#资料分析)
  + [基期与现期](#基期与现期)
  + [增长量](#增长量)
    + [年均增长量](#年均增长量)
  + [增长率](#增长率)
    + [名义增长率与实际增长率](#名义增长率与实际增长率)
    + [拉动增长率](#拉动增长率)
    + [间隔增长率](#间隔增长率)
    + [年均增长率](#年均增长率)
  + [比重](#比重)
    + [基期比重](#基期比重)
    + [两期比重（差）](#两期比重差)
  + [速算](#速算)
    + [百化分](#百化分)
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
r_{\mathrm{avg}} = (\frac{C}{B})^{\frac{1}{n}} - 1
$$

## 比重

### 基期比重

$$
\frac{S}{1+s} \div \frac{T}{1+t} = \frac{S}{T} \cdot \frac{1+t}{1+s}
$$

其中：

+ $S(Segment)$ ：现期部分量
+ $T(Total)$ ：现期整体量
+ $s$ ：部分量的增长率
+ $t$ ：整体量的增长率

### 两期比重（差）

$$
\frac{S}{T} - \frac{S}{T} \cdot \frac{1+t}{1+s} = \frac{S}{T} \cdot \frac{s-t}{1+s}
$$

## 速算

### 百化分

![百化分](images/percent2frac.png)

### 分数比较

> [!TIP]
> 分子快，分子大的分数大
> 
> 分母快，分母大的分数小