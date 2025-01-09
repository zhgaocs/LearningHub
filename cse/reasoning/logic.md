+ [联结词](#联结词)
  + [否定](#否定)
  + [合取](#合取)
  + [析取](#析取)
  + [蕴含](#蕴含)
  + [等价](#等价)
+ [公式等价](#公式等价)

## 联结词

### 否定

> [!TIP]
> $P$：这些都是男生  
> $\neg P$：这些<u>**不都是**</u>男生

|    $P$     |  $\neg P$  |
| :--------: | :--------: |
| $\text{T}$ | $\text{F}$ |
| $\text{F}$ | $\text{T}$ |

### 合取

> [!TIP]
> $P$：2是素数， $Q$：2是偶数  
> $P \land Q$：2是素数，并且是偶数

|    $P$     |    $Q$     | $P \land Q$ |
| :--------: | :--------: | :---------: |
| $\text{T}$ | $\text{T}$ | $\text{T}$  |
| $\text{T}$ | $\text{F}$ | $\text{F}$  |
| $\text{F}$ | $\text{T}$ | $\text{F}$  |
| $\text{F}$ | $\text{F}$ | $\text{F}$  |

### 析取

> [!TIP]
> 析取为“<u>**可兼或**</u>”

|    $P$     |    $Q$     | $P \lor Q$ |
| :--------: | :--------: | :--------: |
| $\text{T}$ | $\text{T}$ | $\text{T}$ |
| $\text{T}$ | $\text{F}$ | $\text{T}$ |
| $\text{F}$ | $\text{T}$ | $\text{T}$ |
| $\text{F}$ | $\text{F}$ | $\text{F}$ |

---

“可兼或”与“排斥或”

【例】

1. 张三爱唱歌或爱听音乐
2. 张三在202或203房间

【解】

1. $P \lor Q$
2. $(U \land \neg V) \lor (\neg U \land V)$

其中：

+ $P$ ：张三爱唱歌， $Q$ ：张三爱听音乐
+ $U$ ：张三在202房间， $V$ ：张三在203房间  

---

### 蕴含

1. 若 $P$ 则 $Q$ ： $P \rightarrow Q$

2. 真值表

|    $P$     |    $Q$     | $P \rightarrow Q$ |
| :--------: | :--------: | :---------------: |
| $\text{T}$ | $\text{T}$ |    $\text{T}$     |
| $\text{T}$ | $\text{F}$ |    $\text{F}$     |
| $\text{F}$ | $\text{T}$ |    $\text{T}$     |
| $\text{F}$ | $\text{F}$ |    $\text{T}$     |

> [!TIP]
> 1. 可以符号化为 $P \rightarrow Q$ 的句式
>     + “如 $P$ ，则 $Q$ ”
>     + “因为 $P$ ，所以 $Q$ ”
>     + “只要 $P$ ，就 $Q$ ”
>     + “ $P$ ，仅当 $Q$ ”
>     + “只有 $Q$，才 $P$ ”
>     + “除非 $Q$，才 $P$ ”
>     + “除非 $Q$，否则非 $P$ ”
> 2. $P \rightarrow Q \Leftrightarrow \neg P \lor Q$  
>     + $P$ ：如果下雨， $Q$ ：我带伞  
>     + $P \rightarrow Q$ ：如果下雨我带伞
>     + $\neg P \lor Q$ ：要么不下雨，要么我带伞

---

【例】

1. 只要天不下雨，我就骑自行车上班
2. 只有天不下雨，我才骑自行车上班
3. 除非今天天气好，否则我不会去公园
4. 我将去镇上，仅当我有时间

【解】

1. $\neg A \rightarrow B$
2. $B \rightarrow \neg A$
3. $Q \rightarrow P$
4. $U \rightarrow V$

其中：
+ 设 $A$ ：天下雨， $B$ ：我骑自行车上班
+ 设 $P$ ：今天天气好， $Q$ ：我去公园
+ 设 $U$ ：我将去镇上， $V$ ：我有时间

---

### 等价

1. $P$ 当且仅当 $Q$ ： $P \leftrightarrow Q$

2. 真值表

|    $P$     |    $Q$     | $P \leftrightarrow Q$ |
| :--------: | :--------: | :-------------------: |
| $\text{T}$ | $\text{T}$ |      $\text{T}$       |
| $\text{T}$ | $\text{F}$ |      $\text{F}$       |
| $\text{F}$ | $\text{T}$ |      $\text{F}$       |
| $\text{F}$ | $\text{F}$ |      $\text{T}$       |

## 公式等价

命题公式 $A$ 和 $B$ 的所有真值全部相同，则称 $A$ 和 $B$ 等值或逻辑等价，记作 $A \Leftrightarrow B$

|  运算律   |                             符号表示                              |
| :-------: | :---------------------------------------------------------------: |
|  结合律   |     $(P \land Q) \land R \Leftrightarrow P \land (Q \land R)$     |
|           |       $(P \lor Q) \lor R \Leftrightarrow P \lor (Q \lor R)$       |
|  分配律   | $P \lor (Q \land R) \Leftrightarrow (P \lor Q) \land (P \lor R)$  |
|           | $P \land (Q \lor R) \Leftrightarrow (P \land Q) \lor (P \land R)$ |
|  吸收律   |               $P \lor(P \land Q) \Leftrightarrow P$               |
|           |               $P \land(P \lor Q) \Leftrightarrow P$               |
| 德•摩根律 |       $\neg (P \lor Q) \Leftrightarrow \neg P \land \neg Q$       |
|           |       $\neg (P \land Q) \Leftrightarrow \neg P \lor \neg Q$       |