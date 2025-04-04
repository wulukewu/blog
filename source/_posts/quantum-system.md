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

本文旨在介紹量子計算 (Quantum Computing) 的基本概念與原理，從量子位元 (Qubit) 的基本概念出發，逐步深入探討多量子位元系統的運算與應用。量子計算與傳統計算 (Classical Computing) 最大的不同在於其底層運算邏輯，透過量子力學的特性，例如疊加 (Superposition) 和糾纏 (Entanglement)，實現傳統計算難以達成的計算能力。

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

#### 基底

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

#### 崩塌 (Collapse)

我們前面說過，$| \psi \rangle = \sum_i \alpha_i | e_i \rangle$ 是由$N$維的基底所組成的。其中 $| e_i \rangle$ 在 $| \psi \rangle$ 出現的機率為 $| \alpha_i |^2$，則$|\alpha_1|^2 + |\alpha_2|^2 + \dots + |\alpha_N|^2 = 1$。

在 $| \psi \rangle = \sum_i \alpha_i | e_i \rangle$ 當中，$| e_i \rangle$ 出現的機率取決於 $| \alpha_i |^2$，而此時的觀測是**不可逆**的。當測量完成後，量子態會崩塌到對應的基底態 $| e_i \rangle$，並且無法回復到原本的疊加態。

這表明測量過程不可逆，且量子態的疊加性在測量後不復存在。

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

### Hilbert 空間與張量積

多量子位系統的 Hilbert 空間是單量子位空間的張量積：

$$
H_2 \otimes H_2 \otimes \dots \otimes H_2 = H_N \quad (N \text{ 個})
$$

假設：

- 系統 1 的 Hilbert 空間 $H_2$：$\{ | 0 \rangle, | 1 \rangle \}$
- 系統 2 的 Hilbert 空間 $H_2$：$\{ | 0 \rangle, | 1 \rangle \}$

則：

$$
| \psi \rangle = \alpha_0 | 0 \rangle + \beta_0 | 1 \rangle, \quad | \psi \rangle = \alpha_1 | 0 \rangle + \beta_1 | 1 \rangle
$$

$$
\langle 0 | 0 \rangle = 0, \quad \langle 0 | 1 \rangle = 0
$$

$$
\langle 0 | 0 \rangle = 1, \quad \langle 1 | 1 \rangle = 1
$$

#### 張量積

$$
H_2 \otimes H_2:
$$

$$
| \psi_1 \rangle \otimes | \psi_0 \rangle = [\alpha_1 | 0 \rangle + \beta_1 | 1 \rangle] \otimes [\alpha_0 | 0 \rangle + \beta_0 | 1 \rangle]
$$

$$
= \alpha_1 \alpha_0 | 0 \rangle \otimes | 0 \rangle + \alpha_1 \beta_0 | 0 \rangle \otimes | 1 \rangle + \beta_1 \alpha_0 | 1 \rangle \otimes | 0 \rangle + \beta_1 \beta_0 | 1 \rangle \otimes | 1 \rangle
$$

$$
= \alpha_1 \alpha_0 | 00 \rangle + \alpha_1 \beta_0 | 01 \rangle + \beta_1 \alpha_0 | 10 \rangle + \beta_1 \beta_0 | 11 \rangle
$$

因此：

- $| 0 \rangle \otimes | 0 \rangle = | 00 \rangle$
- $| 0 \rangle \otimes | 1 \rangle = | 01 \rangle$
- $| 1 \rangle \otimes | 0 \rangle = | 10 \rangle$
- $| 1 \rangle \otimes | 1 \rangle = | 11 \rangle$

##### 範例

假設：

- 系統 1 的 Hilbert 空間 $H_2$：$| 0 \rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $| 1 \rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$
- 系統 2 的 Hilbert 空間 $H_2$：$| 0 \rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $| 1 \rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$

計算張量積：

$$
| 0 \rangle \otimes | 0 \rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix} \otimes \begin{pmatrix} 1 \\ 0 \end{pmatrix} = \begin{pmatrix} 1 \times \begin{pmatrix} 1 \\ 0 \end{pmatrix} \\ 0 \times \begin{pmatrix} 1 \\ 0 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \\ 0 \\ 0 \end{pmatrix} = | 00 \rangle
$$

$$
| 1 \rangle \otimes | 1 \rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix} \otimes \begin{pmatrix} 0 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 \times \begin{pmatrix} 0 \\ 1 \end{pmatrix} \\ 1 \times \begin{pmatrix} 0 \\ 1 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 0 \\ 1 \end{pmatrix} = | 11 \rangle
$$

### 運算子與單元矩陣

#### 運算子

假設運算子 $T$：

$$
T(| \psi \rangle \otimes | \psi \rangle) = \alpha T(| \psi \rangle) + \beta T(| \psi \rangle)
$$

**張量積運算子**：

$$
T_0 \otimes T_1 = T
$$

例如：

$$
I | \psi \rangle = | \psi \rangle
$$

$$
T | \psi_1 \rangle, | \psi_0 \rangle \quad (T \otimes I) (| \psi_1 \rangle \otimes | \psi_0 \rangle)
$$

**單位運算子**：

$$
I \otimes I = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} \otimes \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = \begin{pmatrix} 1 \times \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} & 0 \times \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} \\ 0 \times \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} & 1 \times \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix} = I
$$

#### 單元矩陣 (Unitary Matrix)

假設 $U$ 是單元矩陣：

$$
U = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}
$$

$$
U^\dagger = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}^*
$$

$$
U^\dagger U = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} \cdot \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} = \frac{1}{2} \begin{pmatrix} 2 & 0 \\ 0 & 2 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I
$$

$$
U U^\dagger = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} \cdot \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} = \frac{1}{2} \begin{pmatrix} 2 & 0 \\ 0 & 2 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I
$$

---

## 量子門與狀態轉換 (Quantum Gates and State Transformations)

### 常見量子門

#### Hadamard 門

假設 $H_2$ 具有基底 $I, X, Y, Z$：

- $X | 0 \rangle = | 1 \rangle$, $X | 1 \rangle = | 0 \rangle$, $X = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$, $X^\dagger = X$
- $Y | 0 \rangle = +i | 1 \rangle$, $Y | 1 \rangle = -i | 0 \rangle$, $Y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}$, $Y^\dagger = Y$
- $Z | 0 \rangle = | 0 \rangle$, $Z | 1 \rangle = -| 1 \rangle$, $Z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$, $Z^\dagger = Z$

**Hadamard 門**：

$$
U = \alpha I + \beta X + \gamma Y + \delta Z \quad (\text{Bell 態})
$$

### 糾纏態與測量

#### Bell 態

**Bell 態**：

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

$$
| \psi_1 \rangle \otimes | \psi_0 \rangle = \alpha_1 \alpha_0 | 00 \rangle + \alpha_1 \beta_0 | 01 \rangle + \beta_1 \alpha_0 | 10 \rangle + \beta_1 \beta_0 | 11 \rangle
$$

如果 $\alpha_1 \beta_0 = 0$，則為 **可分離態**；否則為 **糾纏態**。

**糾纏態範例**：

$$
\alpha_1 \alpha_0 | 00 \rangle + \beta_1 \beta_0 | 11 \rangle \neq | \psi_1 \rangle \otimes | \psi_0 \rangle
$$

例如：

$$
| 00 \rangle + | 11 \rangle = | 0 \rangle \otimes | 0 \rangle + | 1 \rangle \otimes | 1 \rangle
$$

#### 糾纏測量

**糾纏測量**：

- 系統 1 測量到 $| 0 \rangle$，則系統 2 為 $| 0 \rangle$
- 系統 1 測量到 $| 1 \rangle$，則系統 2 為 $| 1 \rangle$

### 逆向計算 (Reverse Computation)

#### CNOT (Control NOT)

CNOT 門的運作如下：

![CNOT Gate](images/cnot_gate.png)

- $0 \rightarrow 0$, $1 \rightarrow 1$, $| 0 \rangle \otimes | 0 \rangle = 1$
- $1 \rightarrow 1$, $1 \rightarrow 0$, $| 0 \rangle \otimes | 0 \rangle = 0$

#### SWAP 門

SWAP 門的運作如下：

![SWAP Gate](images/swap_gate.png)

#### CCNOT (Toffoli Gate)

CCNOT 門的運作如下：

![CCNOT Gate](images/ccnot_gate.png)

- $a=0, b=0 \implies ab=0 \implies | c \oplus ab \rangle = | c \oplus 0 \rangle = | c \rangle$
- $a=1, b=1 \implies ab=1 \implies | c \oplus ab \rangle = | c \oplus 1 \rangle = | \overline{c} \rangle$

**CCNOT 性質**：

$$
\text{CCNOT} \cdot \text{CCNOT} = I
$$

$$
\text{CCNOT}^\dagger \cdot \text{CCNOT} = I
$$

$$
\text{CCNOT}^\dagger = \text{CCNOT}
$$

### 邏輯運算門

#### AND 門

AND 門的運作如下：

![AND Gate](images/and_gate.png)

#### XOR 門

XOR 門的運作如下：

![XOR Gate](images/xor_gate.png)

#### NAND 門

NAND 門的運作如下：

![NAND Gate](images/nand_gate.png)

#### OR 門

OR 門的運作如下：

![OR Gate](images/or_gate.png)

##### 範例

以下是量子電路的等價性：

![Quantum Circuit Equivalence](images/quantum_circuit_equivalence.png)

---

## 結論

至此介紹了量子計算的基礎，從單量子位系統的 Hilbert 空間、內積、正規化，到多量子位系統的張量積、糾纏態，以及量子門和狀態轉換。
