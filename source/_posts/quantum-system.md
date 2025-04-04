---
title: 量子計算基礎 - 從單量子位到多量子位系統
date: 2025-04-04 18:48:02
tags:
  - quantum
  - hilbert-space
  - algorithms
  - physics
mathjax: true
---

（大一下去修了一門量子計算的課程，跟一般電路學不一樣的是引入了新設計的邏輯閘。）

# 量子計算基礎

本文會介紹量子計算 (Quantum Computing) 的基本概念與原理，從量子位元 (Qubit) 的基本概念出發，逐步深入探討多量子位元系統的運算與應用。量子計算與傳統計算 (Classical Computing) 最大的不同在於其底層運算邏輯，透過量子力學的特性，例如疊加 (Superposition) 和糾纏 (Entanglement)，實現傳統計算難以達成的計算能力。

---

## 單量子位系統 (Single-Qubit Quantum Systems)

在量子計算中，量子位元 (Qubit) 是最基本的資訊單位，類似於傳統計算中的位元 (Bit)。然而，與傳統位元只能處於 0 或 1 的狀態不同，量子位元可以同時處於 0 和 1 的疊加態。本節將介紹單量子位元系統的基本概念，包括描述量子態的 Hilbert 空間 (Hilbert Space)、向量空間 (Vector Space) 的數學表示，以及相關的運算。

### Hilbert 空間 (Hilbert Space)

在量子力學中，量子態並非存在於我們熟悉的歐幾里得空間，而是存在於一個更抽象的數學空間，稱為 Hilbert 空間。**因此，理解 Hilbert 空間對於理解量子態至關重要。** Hilbert 空間是一個完備的內積空間，允許我們進行向量的加法、純量乘法，以及計算向量之間的內積。對於單量子位元系統，Hilbert 空間是一個複數向量空間，具有以下特性：

- 量子態可以表示為 Hilbert 空間中的向量，例如 $| \psi \rangle$ 和 $| \phi \rangle$，它們都屬於 Hilbert 空間 $V$，即 $| \psi \rangle, | \phi \rangle \in V$。其中，$\alpha$ 和 $\beta$ 是複數，即 $\alpha, \beta \in \mathbb{C}$。
- Hilbert 空間滿足線性疊加原理，即如果 $| \psi \rangle$ 和 $| \phi \rangle$ 是 Hilbert 空間中的向量，那麼它們的線性組合 $\alpha | \psi \rangle + \beta | \phi \rangle$ 也屬於 Hilbert 空間 $V$，即 $\alpha | \psi \rangle + \beta | \psi \rangle \in V$。

假設 Hilbert 空間的基底為 $\{ | e_1 \rangle, | e_2 \rangle, \dots, | e_N \rangle \}$，對於維度 (Dimension) 為 $N$ 的向量空間，任意量子態 $| \psi \rangle$ 都可以表示為基底向量的線性組合：

$$
| \psi \rangle = \sum_i \alpha_i | e_i \rangle, \quad \alpha_i \in \mathbb{C}
$$

其中，$\alpha_i$ 是複數，表示基底向量 $| e_i \rangle$ 在量子態 $| \psi \rangle$ 中的權重 (Weight)。基底向量 $| e_i \rangle$ 可以表示為列向量：

$$|e_1\rangle = \begin{pmatrix} 1 \\ 0 \\ \vdots \\ 0 \end{pmatrix}, |e_2\rangle = \begin{pmatrix} 0 \\ 1 \\ \vdots \\ 0 \end{pmatrix}, \ldots, |e_N\rangle = \begin{pmatrix} 0 \\ \vdots \\ 0 \\ 1 \end{pmatrix}$$

**Note**：單量子位元系統的 Hilbert 空間是一個 $N=2$ 的簡單空間，而複數量子位元系統的 Hilbert 空間維度會隨著量子位元數量增加而指數成長，例如 5 個量子位元系統的 Hilbert 空間維度為 $N=2^5=32$。

##### 範例

我們拿一個例子來說明好了，假設 $| \psi \rangle = \begin{pmatrix} 1-i \\ 2 \end{pmatrix}$, $| \phi \rangle = \begin{pmatrix} 1 \\ 1+i \end{pmatrix}$，那麼可以做下列這幾個運算：

#### 對偶向量 (Dual Vector)

**在進行內積運算之前，我們需要先了解對偶向量的概念。**

- $\langle \psi | = \begin{pmatrix} 1-i & 2 \end{pmatrix}^* = \begin{pmatrix} 1+i & 2 \end{pmatrix}$
- $\langle \phi | = \begin{pmatrix} 1 & 1+i \end{pmatrix}^* = \begin{pmatrix} 1 & 1-i \end{pmatrix}$

#### 內積 (Inner Product)

**有了對偶向量的基礎，我們就可以計算向量的內積。**

$$
\langle \phi | \psi \rangle = \begin{pmatrix} 1 & 1-i \end{pmatrix} \begin{pmatrix} 1-i \\ 2 \end{pmatrix} = 3(1-i)
$$

$$
\langle \psi | \phi \rangle = \begin{pmatrix} 1+i & 2 \end{pmatrix} \begin{pmatrix} 1 \\ 1+i \end{pmatrix} = 3(1+i)
$$

$$
\langle \psi | \psi \rangle = \begin{pmatrix} 1+i & 2 \end{pmatrix} \begin{pmatrix} 1-i \\ 2 \end{pmatrix} = 6 \quad (\in \mathbb{R}_{\geq 0})
$$

$$
\langle \phi | \phi \rangle = \begin{pmatrix} 1 & 1-i \end{pmatrix} \begin{pmatrix} 1 \\ 1+i \end{pmatrix} = 3 \quad (\in \mathbb{R}_{\geq 0})
$$

因此：

- $\langle \phi | \psi \rangle \neq \langle \psi | \phi \rangle^*$，表示 $\langle \phi | \psi \rangle \neq \langle \psi | \phi \rangle$
- $\langle \psi | \psi \rangle = 0 \implies | \psi \rangle$ 是正交 (Orthogonal)

#### 向量範數 (Norm)

向量範數 (Norm) 是一種衡量向量大小的函數，它將向量映射到一個非負實數。在量子力學中，向量範數用於衡量量子態的長度。

$$
\| \psi \| = \sqrt{\langle \psi | \psi \rangle}
$$

#### 正規化向量 (Normalized Vector)

正規化向量 (Normalized Vector) 是指將向量縮放到長度為 1 的過程。在量子力學中，量子態必須是正規化的，以保證測量結果的機率總和為 1。

$$
| \psi \rangle_N = \frac{| \psi \rangle}{\| \psi \|}
$$

例如：

$$
| \psi \rangle = \frac{1}{\sqrt{6}} \begin{pmatrix} 1-i \\ 2 \end{pmatrix}, \quad | \phi \rangle = \frac{1}{\sqrt{3}} \begin{pmatrix} 1 \\ 1+i \end{pmatrix}
$$

計算內積：

$$
\langle \psi | \psi \rangle = \frac{\begin{pmatrix} 1+i & 2 \end{pmatrix}}{\sqrt{6}} \cdot \frac{\begin{pmatrix} 1-i \\ 2 \end{pmatrix}}{\sqrt{6}} = \frac{6}{6} = 1
$$

$$
\langle \phi | \phi \rangle = \frac{\begin{pmatrix} 1 & 1-i \end{pmatrix}}{\sqrt{3}} \cdot \frac{\begin{pmatrix} 1 \\ 1+i \end{pmatrix}}{\sqrt{3}} = \frac{3}{3} = 1
$$

### 基底與投影運算子

在量子力學中，基底 (Basis) 是一組線性無關的向量，可以張成整個 Hilbert 空間。任意量子態都可以表示為基底向量的線性組合。投影運算子 (Projection Operator) 則是用於將量子態投影到特定基底向量上的運算符。本節將介紹基底與投影運算子的基本概念與性質。

#### 基底 (Basis)

基底 $\{ | e_1 \rangle, | e_2 \rangle, \dots, | e_N \rangle \}$ 滿足：

- **正規化 (Normalized)**：$\langle e_i | e_i \rangle = \langle e_2 | e_2 \rangle = \dots = \langle e_N | e_N \rangle = 1$
- **正交 (Orthogoral)**：$\langle e_i | e_j \rangle = \langle e_2 | e_3 \rangle = \dots = \langle e_N | e_N \rangle = 0$

因此：

$$
\langle e_i | e_j \rangle = \delta_{ij} = \begin{cases} 1 & \text{if } i = j \\ 0 & \text{if } i \neq j \end{cases}
$$

##### 範例

假設基底：$| e_1 \rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $| e_2 \rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$

那麼可以先求出對偶向量：$\langle e_1 | = \begin{pmatrix} 1 & 0 \end{pmatrix}, \langle e_2 | = \begin{pmatrix} 0 & 1 \end{pmatrix}$

計算：

$$
\langle e_1 | e_1 \rangle = \begin{pmatrix} 1 & 0 \end{pmatrix} \begin{pmatrix} 1 \\ 0 \end{pmatrix} = 1
$$

$$
\langle e_2 | e_2 \rangle = \begin{pmatrix} 0 & 1 \end{pmatrix} \begin{pmatrix} 0 \\ 1 \end{pmatrix} = 1
$$

#### 投影運算子 (Projection Operator)

$$
| \psi \rangle = \frac{1}{\sqrt{6}} \begin{pmatrix} 1-i \\ 2 \end{pmatrix} = \frac{1-i}{\sqrt{6}} | e_1 \rangle + \frac{2}{\sqrt{6}} | e_2 \rangle
$$

$$
\langle e_1 | \psi \rangle = \langle e_1 | \left( \frac{1-i}{\sqrt{6}} | e_1 \rangle + \frac{2}{\sqrt{6}} | e_2 \rangle \right) = \frac{1-i}{\sqrt{6}} \langle e_1 | e_1 \rangle + \frac{2}{\sqrt{6}} \langle e_1 | e_2 \rangle = \frac{1-i}{\sqrt{6}}
$$

$$
\langle e_2 | \psi \rangle = \langle e_2 | \left( \frac{1-i}{\sqrt{6}} | e_1 \rangle + \frac{2}{\sqrt{6}} | e_2 \rangle \right) = \frac{1-i}{\sqrt{6}} \langle e_2 | e_1 \rangle + \frac{2}{\sqrt{6}} \langle e_2 | e_2 \rangle = \frac{2}{\sqrt{6}}
$$

此時算出的 $\langle e_1 | \psi \rangle = \frac{1-i}{\sqrt{6}}$ 和 $\langle e_2 | \psi \rangle = \frac{2}{\sqrt{6}}$ 就分別是 $| \psi \rangle$ 在 $| e_1 \rangle$ 和 $| e_2 \rangle$ 兩個基底的投影運算子。

#### 崩塌 (Collapse)

我們前面說過，$| \psi \rangle = \sum_i \alpha_i | e_i \rangle$ 是由$N$維的基底所組成的。其中 $| e_i \rangle$ 在 $| \psi \rangle$ 出現的機率為 $| \alpha_i |^2$，則$|\alpha_1|^2 + |\alpha_2|^2 + \dots + |\alpha_N|^2 = 1$。

在 $| \psi \rangle = \sum_i \alpha_i | e_i \rangle$ 當中，$| e_i \rangle$ 出現的機率取決於 $| \alpha_i |^2$，而此時的觀測是**不可逆**的。當測量完成後，量子態會崩塌到對應的基底態 $| e_i \rangle$，並且無法回復到原本的疊加態。因此**測量過程不可逆，且量子態的疊加性在測量後不復存在**。

##### 範例

$$
| \psi \rangle = \frac{1}{\sqrt{6}} \begin{pmatrix} 1-i \\ 2 \end{pmatrix} = \frac{1-i}{\sqrt{6}} | e_1 \rangle + \frac{2}{\sqrt{6}} | e_2 \rangle
$$

- $| e_1 \rangle$ 在 $| \psi \rangle$ 出現的機率為 $\left| \frac{1-i}{\sqrt{6}} \right|^2 = \frac{2}{6} = \frac{1}{3}$

- $| e_2 \rangle$ 在 $| \psi \rangle$ 出現的機率為 $\left| \frac{2}{\sqrt{6}} \right|^2 = \frac{4}{6} = \frac{2}{3}$

- $\frac{1}{3} + \frac{2}{3} = 1$

#### 量子態 (Quantum State)：

$$
P_i | \psi \rangle \rightarrow | e_i \rangle
$$

$$
| \psi \rangle = \alpha_1 | e_1 \rangle + \alpha_2 | e_2 \rangle
$$

#### 經典態 (Classical State)：

$$
\frac{P_1}{| \alpha_1 |^2} | \psi \rangle = \frac{\alpha_1}{| \alpha_1 |^2} | e_1 \rangle = e^{i\theta_1} | e_1 \rangle
$$

$$
\frac{P_2}{| \alpha_2 |^2} | \psi \rangle = \frac{\alpha_2}{| \alpha_2 |^2} | e_2 \rangle = e^{i\theta_2} | e_2 \rangle
$$

### Bloch 球 (Bloch Sphere)

Bloch 球用於表示單量子位的狀態：

![Bloch Sphere](../../../images/posts/quantum-system/bloch_sphere.png)

- $| 0 \rangle \rightarrow (0, 0, 1)$
- $| 1 \rangle \rightarrow (0, 0, -1)$
- $| + \rangle \rightarrow (1, 0, 0)$
- $| - \rangle \rightarrow (-1, 0, 0)$
- $| i \rangle \rightarrow (0, 1, 0)$
- $| -i \rangle \rightarrow (0, -1, 0)$

---

## 多量子位系統 (Multiple-Qubit Systems)

### Hilbert 空間與張量積 (Tensor Product)

多量子位系統的 Hilbert 空間是單量子位空間的張量積：

$$
H_2 \otimes H_2 \otimes \dots \otimes H_2 = H_N \quad (N \text{ 個})
$$

假設：

- 第 0 個 $H_2$：$\{ | 0 \rangle _0, | 1 \rangle _0 \}$
- 第 1 個 $H_2$：$\{ | 0 \rangle _1, | 1 \rangle _1 \}$

則：

$$
| \psi_{0} \rangle = \alpha_{0} | 0 \rangle_{0} + \beta_{0} | 1 \rangle_{0}, \quad | \psi_{1} \rangle = \alpha_{1} | 0 \rangle_{1} + \beta_{1} | 1 \rangle_{1}
$$

$$
{}_{0}\langle 0 | 1 \rangle_{0} = 0, \quad {}_{1}\langle 0 | 1 \rangle_{1} = 0
$$

$$
{}_{0}\langle 0 | 0 \rangle_{0} = {}_{0}\langle 1 | 1 \rangle_{0} = 1, \quad {}_{1}\langle 0 | 0 \rangle_{1} = {}_{1}\langle 1 | 1 \rangle_{1} = 1
$$

#### 張量積

$H_2 \otimes H_2:$

$$
| \psi_1 \rangle \otimes | \psi_0 \rangle = [\alpha_1 | 0 \rangle_1 + \beta_1 | 1 \rangle_1] \otimes [\alpha_0 | 0 \rangle_0 + \beta_0 | 1 \rangle_0]
$$

$$
= \alpha_1 \alpha_0 | 0 \rangle_1 \otimes | 0 \rangle_0 + \alpha_1 \beta_0 | 0 \rangle_1 \otimes | 1 \rangle_0 + \beta_1 \alpha_0 | 1 \rangle_1 \otimes | 0 \rangle_0 + \beta_1 \beta_0 | 1 \rangle_1 \otimes | 1 \rangle_0
$$

$$
= \alpha_1 \alpha_0 | 00 \rangle_{10} + \alpha_1 \beta_0 | 01 \rangle_{10} + \beta_1 \alpha_0 | 10 \rangle_{10} + \beta_1 \beta_0 | 11 \rangle_{10}
$$

而此時：

- $| 0 \rangle_1 \otimes | 0 \rangle_0 = | 00 \rangle_{10}$
- $| 0 \rangle_1 \otimes | 1 \rangle_0 = | 01 \rangle_{10}$
- $| 1 \rangle_1 \otimes | 0 \rangle_0 = | 10 \rangle_{10}$
- $| 1 \rangle_1 \otimes | 1 \rangle_0 = | 11 \rangle_{10}$

$| 00 \rangle, | 01 \rangle, | 10 \rangle, | 11 \rangle$ 為 $H_4$ 的基底

##### 範例

假設：

- 第0個 $H_2$：$| 0 \rangle _0 = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $| 1 \rangle _0 = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$, $T_0$ operator
- 第1個 $H_2$：$| 0 \rangle _1 = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $| 1 \rangle _1 = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$, $T_1$ operator

計算張量積：

$$
| 0 \rangle _{10} = |00 \rangle _2 = | 0 \rangle _1 \otimes |0 \rangle _0 = \begin{pmatrix} 1 \\ 0 \end{pmatrix} \otimes \begin{pmatrix} 1 \\ 0 \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \\ 0 \\ 0 \end{pmatrix}
$$

$$
| 1 \rangle _{10} = |01 \rangle _2 = | 0 \rangle _1 \otimes |1 \rangle _0 = \begin{pmatrix} 1 \\ 0 \end{pmatrix} \otimes \begin{pmatrix} 0 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 \\ 1 \\ 0 \\ 0 \end{pmatrix}
$$

$$
| 2 \rangle _{10} = |10 \rangle _2 = | 1 \rangle _1 \otimes |0 \rangle _0 = \begin{pmatrix} 0 \\ 1 \end{pmatrix} \otimes \begin{pmatrix} 1 \\ 0 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 1 \\ 0 \end{pmatrix}
$$

$$
| 3 \rangle _{10} = |11 \rangle _2 = | 1 \rangle _1 \otimes |1 \rangle _0 = \begin{pmatrix} 0 \\ 1 \end{pmatrix} \otimes \begin{pmatrix} 0 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 0 \\ 1 \end{pmatrix}
$$

### 運算子與單元矩陣

在多量子位系統中，運算子 $T$ 和單位運算子 $I$ 的結合可以用來描述量子態的演化。假設 $T_0$ 和 $T_1$ 是作用於不同量子位的運算子，若它們相等，即 $T_0 = T_1 = T$，則可以簡化為單一運算子 $T$ 的作用。

單位運算子 $I$ 的作用不會改變量子態，滿足以下關係：

$$
I |\psi\rangle = |\psi\rangle
$$

其中，單位運算子 $I$ 的矩陣形式為：

$$
I = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}
$$

當運算子 $T$ 作用於單一量子位的量子態時，可以表示為：

$$
T | \psi _1 \rangle, | \psi _0 \rangle
$$

而當運算子 $T$ 與單位運算子 $I$ 結合，作用於多量子位系統的張量積態時，則可以表示為：

$$
(T \otimes I) (| \psi _1 \rangle \otimes | \psi _0 \rangle)
$$

這種表示方式清楚地展示了運算子如何作用於多量子位系統，並且是量子計算中描述量子態演化的重要工具。

**單位運算子**：

$$
I \otimes I = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} \otimes \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = \begin{pmatrix} 1 \times \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} & 0 \times \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} \\ 0 \times \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} & 1 \times \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix} = I
$$

#### 單元矩陣 (Unitary Matrix)

假設 $U$ ：

$$
U = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & i \\ i & 1 \end{pmatrix}
$$

則可發現 $U^{-1} = U^\dagger$

**Note**：共軛轉置 $U^\dagger = (U^*)^T$

$$
U^{-1} = [\frac{1}{\sqrt{2}} \begin{pmatrix} 1 & i \\ i & 1 \end{pmatrix}]^* = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix}
$$

$$
U^\dagger U = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix} \cdot \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & i \\ i & 1 \end{pmatrix} = \frac{1}{2} \begin{pmatrix} 2 & 0 \\ 0 & 2 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I
$$

$$
U U^\dagger = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & i \\ i & 1 \end{pmatrix} \cdot \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix} = \frac{1}{2} \begin{pmatrix} 2 & 0 \\ 0 & 2 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I
$$

$U$ 是單元矩陣

---

## 量子門與狀態轉換 (Quantum Gates and State Transformations)

### 常見量子門

$H_2$ 的基本運算子為 $I, X, Y, Z$

#### 基本運算子 - X (NOT)

$$
X | 0 \rangle = | 1 \rangle, \quad X | 1 \rangle = | 0 \rangle
$$

$$
X = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}
$$

其中：

$$
X^2 = I = X^{-1}X
$$

$$
X^{-1} = X
$$

#### 基本運算子 - Y
$$
Y | 0 \rangle = +i | 1 \rangle, \quad Y | 1 \rangle = -i | 0 \rangle
$$

$$
Y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}
$$

其中：
$$
Y^\dagger = Y
$$

$$
Y^\dagger Y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I
$$

$$
Y^2 = I
$$

$Y$ 是單元矩陣

#### 基本運算子 - Z

$$
Z | 0 \rangle = | 0 \rangle, \quad Z | 1 \rangle = -| 1 \rangle
$$

$$
Z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
$$

### 糾纏態與測量

在$H_2$中，

$$
U = \alpha I + \beta X + \gamma Y + \delta Z
$$

#### Bell State （貝爾態）

$$
\frac{1}{\sqrt{2}} (| 00 \rangle + | 11 \rangle)
$$

$$
\frac{1}{\sqrt{2}} (| 00 \rangle - | 11 \rangle)
$$

$$
\frac{1}{\sqrt{2}} (| 01 \rangle + | 10 \rangle)
$$

$$
\frac{1}{\sqrt{2}} (| 01 \rangle - | 10 \rangle)
$$

假設：

$$
| \psi_1 \rangle = \alpha_1 | 0 \rangle + \beta_1 | 1 \rangle, \quad | \psi_0 \rangle = \alpha_0 | 0 \rangle + \beta_0 | 1 \rangle
$$

則它們的張量積

$$
| \psi_1 \rangle \otimes | \psi_0 \rangle = \alpha_1 \alpha_0 | 00 \rangle + \alpha_1 \beta_0 | 01 \rangle + \beta_1 \alpha_0 | 10 \rangle + \beta_1 \beta_0 | 11 \rangle
$$

如果 $\beta_1 \alpha_0 = \alpha_1 \beta_0 = 0$，則為 **可分離態**；否則為 **糾纏態 (entanglement)**。

#### 糾纏測量

$$
\alpha_1 \alpha_0 | 00 \rangle + \beta_1 \beta_0 | 11 \rangle \neq | \psi_1 \rangle \otimes | \psi_0 \rangle
$$

例如：

$$
| 00 \rangle + | 11 \rangle = | 0 \rangle _1 \otimes | 0 \rangle _0 + | 1 \rangle _1 \otimes | 1 \rangle _0
$$

![Entangled Measurement](../../../images/posts/quantum-system/entangled_measurement.png)

此時去做量子測量：

- 第 1 個質點測量到 $| 0 \rangle$，則第 0 個質點就確定為 $| 0 \rangle$
- 第 1 個質點測量到 $| 1 \rangle$，則第 0 個質點就確定為 $| 1 \rangle$

### 逆向計算 (Reverse Computation)

#### CNOT (Control NOT)

CNOT 門的運作如下：

![CNOT Gate](../../../images/posts/quantum-system/cnot_gate.png)

$$
\text{CNOT} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix} = \begin{pmatrix} I & O \\ O & X \end{pmatrix}
$$

**CNOT 性質**：

$$
\text{CNOT} \cdot \text{CNOT} = I
$$

$$
\text{CNOT}^{-1} \cdot \text{CNOT} = I
$$

$$
\text{CNOT}^{-1} = \text{CNOT}
$$

舉個例子好了：

| $1 \otimes 0 = 1$ | $1 \otimes 1 = 0$ |
|-----------|-----------|
| ![Example 1](../../../images/posts/quantum-system/cnot_gate_ex1.png) | ![Example 2](../../../images/posts/quantum-system/cnot_gate_ex2.png) |

這是所有計算中最重要的一個運算門，並且可以延伸出 COPY、NOT 和 SWAP 三種操作：

| COPY | NOT | SWAP |
|-----------|-----------|-----------|
| ![COPY Gate](../../../images/posts/quantum-system/cnot_gate_copy.png) | ![NOT Gate](../../../images/posts/quantum-system/cnot_gate_not.png) | ![SWAP Gate (Using CNOT)](../../../images/posts/quantum-system/swap_gate_cnot.jpg) |

#### CCNOT (Toffoli Gate)

CCNOT 門的運作如下：

![CCNOT Gate](../../../images/posts/quantum-system/ccnot_gate.png)

- $a=0, b=0 \implies ab=0 \implies | c \oplus ab \rangle = | c \oplus 0 \rangle = | c \rangle$
- $a=1, b=1 \implies ab=1 \implies | c \oplus ab \rangle = | c \oplus 1 \rangle = | \overline{c} \rangle$

**CCNOT 性質**：

![CCNOT Reversibility](../../../images/posts/quantum-system/ccnot_gate_reversibility.png)

$$
\text{CCNOT} \cdot \text{CCNOT} = I
$$

$$
\text{CCNOT}^{-1} \cdot \text{CCNOT} = I
$$

$$
\text{CCNOT}^{-1} = \text{CCNOT}
$$

### 邏輯運算門

#### AND

AND 的運作如下：

![AND Gate](../../../images/posts/quantum-system/and_gate.png)

#### XOR

XOR 的運作如下：

![XOR Gate](../../../images/posts/quantum-system/xor_gate.png)

#### NAND (NOT AND)

NAND 的運作如下：

![NAND Gate](../../../images/posts/quantum-system/nand_gate.png)

#### NOT

NOT 也可以用 CNOT 的形式來表示：

![NOT Gate](../../../images/posts/quantum-system/not_gate.png)

#### OR

OR 的運作如下：

![OR Gate](../../../images/posts/quantum-system/or_gate.png)

##### 範例

以下是量子電路的等價性：

![Quantum Circuit Equivalence](../../../images/posts/quantum-system/quantum_circuit_equivalence.png)

---

## 結論

至此介紹了量子計算的基礎，從單量子位系統的 Hilbert 空間、內積、正規化，到多量子位系統的張量積、糾纏態，以及量子門和狀態轉換。
