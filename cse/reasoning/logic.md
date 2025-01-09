+ [命题及符号化](#命题及符号化)
  + [联结词](#联结词)
    + [否定](#否定)
    + [合取](#合取)
    + [析取](#析取)
    + [蕴含](#蕴含)
    + [等价](#等价)
+ [命题公式](#命题公式)
  + [公式等价](#公式等价)

## 命题及符号化

### 联结词

#### 否定

|  $P$  | $\neg P$ |
| :---: | :------: |
|   T   |    F     |
|   F   |    T     |

> [!TIP]
> $P$：这些都是男生  
> $\neg P$：这些<u>**不都是**</u>男生

#### 合取

|  $P$  |  $Q$  | $P \land Q$ |
| :---: | :---: | :---------: |
|   T   |   T   |      T      |
|   T   |   F   |      F      |
|   F   |   T   |      F      |
|   F   |   F   |      F      |

> [!TIP]
> $P$：2是素数， $Q$：2是偶数  
> $P \land Q$：2是素数，并且是偶数

#### 析取

|  $P$  |  $Q$  | $P \lor Q$ |
| :---: | :---: | :--------: |
|   T   |   T   |     T      |
|   T   |   F   |     T      |
|   F   |   T   |     T      |
|   F   |   F   |     F      |

> [!TIP]
> “可兼或”与“排斥或”

#### 蕴含

$P \rightarrow Q$

+ $P$ 蕴含 $Q$
+ 如果 $P$ ，那么 $Q$
+ 若 $P$ 则 $Q$ 

|  $P$  |  $Q$  | $P \rightarrow Q$ |
| :---: | :---: | :---------------: |
|   T   |   T   |         T         |
|   T   |   F   |         F         |
|   F   |   T   |         T         |
|   F   |   F   |         T         |

#### 等价

$P \leftrightarrow Q$

|  $P$  |  $Q$  | $P \leftrightarrow Q$ |
| :---: | :---: | :-------------------: |
|   T   |   T   |           T           |
|   T   |   F   |           F           |
|   F   |   T   |           F           |
|   F   |   F   |           T           |

## 命题公式

### 公式等价

命题公式$A$和$B$的所有真值全部相同，则称$A$和$B$等值或逻辑等价，记作$A \Leftrightarrow B$

|  运算律  |                             符号表示                              |
| :------: | :---------------------------------------------------------------: |
|  结合律  |     $(P \land Q) \land R \Leftrightarrow P \land (Q \land R)$     |
|          |       $(P \lor Q) \lor R \Leftrightarrow P \lor (Q \lor R)$       |
|  分配律  | $P \lor (Q \land R) \Leftrightarrow (P \lor Q) \land (P \lor R)$  |
|          | $P \land (Q \lor R) \Leftrightarrow (P \land Q) \lor (P \land R)$ |
|  吸收律  |                                                                   |
| 德摩根律 |                                                                   |