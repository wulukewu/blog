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

大一下去修了一門量子計算的課，前面的概念跟線性代數有滿多相似的地方，後半部分才真正開始講量子演算法。

## 量子計算基礎簡介

量子電腦與傳統電腦的差別，在於傳統電腦儲存資訊的最小單位是位元（bit），量子電腦則是使用量子位元（qubit）。位元可以存在一種狀態，1 或是 0。量子位元特別的地方是，它在一個時間，可以同時是 1 也是 0。

![bit vs qubit](https://image-cdn.learnin.tw/bnextmedia/image/album/2024-04/img-1713263203-31716.jpg?output=webp&w=1200)

量子在通訊上有很高的安全性，利用量子力學原理，能夠在兩方之間安全地分發加密密鑰。任何試圖竊聽的行為都會擾亂量子態，被接收方檢測到。

![quantum application](https://image-cdn.learnin.tw/bnextmedia/image/album/2024-04/img-1713263155-27502.jpg?output=webp&w=1200)

過去超大整數的質因數分解，即使傳統超級電腦的運算能力也無法在短時間破解。不過，量子演算法（Shor's Algorithm，可解質因數分解）能在合理時間內完成破解，會顛覆現在 RSA 等加密算法。

### Classical v.s. Quantum

#### 拆解質數

現在有個數字 $N = f_1 \times f_2$，由 $f_1, f_2$ 兩個很大的質數構成。破解 RSA 的核心，就是從 $N$ 找出 $f_1$ 和 $f_2$。

想要找到 $N$ 的因數，只要不斷給定 $g$，透過 Euclid's Algorithm（歐幾里得演算法，又稱輾轉相除法）快速計算判斷，當 $g$ 使得公因數 $\gcd(N, g) = a > 1$ 時，對於 RSA 來說就已經結束了。

但要找到 $g$ 可以滿足上述條件其實並不容易，事實上真的只能一個一個猜 $g$ 是多少。不過，我們可以將這個隨機猜測的數字 $g$ 轉換成很有可能滿足條件的 $g^{p/2} \pm 1$。

<details>
<summary>為什麼是 $g^{p/2} \pm 1$ ？</summary>

若給定兩個數 $A, B$，且 $\gcd(A, B) = 1$，則存在一個正整數 $p$ 使得 $A^p = m \cdot B + 1$，其中 $m$ 為某個整數。

舉兩個例子來說：

**Ex1**

給定 $(A, B) = (7, 15)$，則：

$$
\begin{align*}
p &= 2, & 7^2 &= 3 \cdot 15 + 4 \\
p &= 3, & 7^3 &= 22 \cdot 15 + 13 \\
p &= 4, & 7^4 &= 160 \cdot 15 + 1
\end{align*}
$$

**Ex2**

給定 $(A, B) = (42, 13)$，則：

$$
\begin{align*}
p &= 2, & 42^2 &= 135 \cdot 13 + 9 \\
p &= 3, & 42^3 &= 5699 \cdot 13 + 1
\end{align*}
$$

因此，$m \cdot B = A^p - 1 = \left(A^{p/2}+1\right)\left(A^{p/2}-1\right)$

</details>

將機會不大的數字 g 轉換成很有可能的$g^{p/2} \pm 1$，只要找到這樣的$p$就好（$p$要是偶數才能真正拆解喔！）

#### Classical

我們用個例子來想，要用傳統電腦找到一個 $p$ 使得 $42^p = m \times 13 + 1$，可能會從 $p=1,2,3,\ldots$ 開始一個一個慢慢代入判斷，但如果現在給定 $g^p = m \times N + 1$ 的 $g$ 和 $N$ 都很大呢？

對傳統電腦來說，就真的只能一個一個數字慢慢算，直到找到答案為止，這也就是為什麼現在能夠這麼廣泛地使用 RSA 加密。但是對量子電腦來說就不太一樣了……

#### Quantum

用量子來計算的好處是因為有**疊加態（superposition）**。

我們可以構建一個 $f(x)$ 的函數。若要計算 $a, b, c, d$ 各自的函數值，對於傳統電腦來說，就是一個一個代入計算：

$$
f(a),\quad f(b),\quad f(c),\quad f(d)
$$

但在量子的世界，可以讓一個或多個量子位元處於疊加態：

$$
|a\rangle + |b\rangle + |c\rangle + |d\rangle
$$

然後將這個疊加態輸入設計好的 $f(x)$ 邏輯閘做平行運算，得到：

$$
|f(a)\rangle + |f(b)\rangle + |f(c)\rangle + |f(d)\rangle
$$

---

到這邊還需要先知道另一個 Shor's Algorithm 的核心概念。一樣目標是找到滿足條件的 $p$。如果現在隨便找的一個 $x$，會得到 $g^x = mN + r$，其中 $r$ 是餘數，那麼很容易證明 $g^{x+p} = m_2 N + r$ 也成立。

換句話說，對於某個週期 $p$，$g^x$ 模 $N$ 的餘數會重複出現：

$$
g^x \bmod N = r \implies g^{x+p} \bmod N = r
$$

這個「週期性」就是 Shor's Algorithm 能有效率分解質因數的關鍵。只要能找到這個週期 $p$，就能透過 Quantum Fourier Transform（量子傅立葉變換）進一步拆解 $N$。

![quantum fourier transform](../../../images/posts/quantum-system/quantum_fourier_transform.png)

---

前面提到，在量子的世界中，我們可以構建量子閘，讓所有輸入 $x$ 的餘數 $r$ 同時被計算出來：

$$
|x\rangle \longrightarrow |x\rangle |g^x \bmod N\rangle
$$

這表示，當量子位元處於疊加態時，經過適當設計的量子閘後，所有 $x$ 對應的 $g^x \bmod N$ 都會同時存在於量子態中。

![quantum shor process](../../../images/posts/quantum-system/quantum_shor_process.png)

從這些餘數當中，任取一個 $r$，可以經由適當的轉換，將其餘的 $x$ 都變成 $0$。最後透過 Fourier Transform 找出的 $g = x$ 和 $p$，就可以利用前面提過的傳統方式，判斷 $g^{p/2} \pm 1$ 是否與 $N$ 有公因數，將 $N$ 拆解開來。

---

> 標準的 2048 位元 RSA 加密，就算用目前世界上最強的超級電腦（太湖之光，中國製），花費地球年齡的時間（46 億年）都無法破解。

如果量子電腦真的存在，能將運算時間由數十億年縮減為幾分、幾秒鐘，數字 $N$ 都能快速被拆解成 $f_1, f_2$ 兩個質數，現在的金融、通訊等都會受到嚴重的影響。但是現在還不需要擔心，因為目前的技術還沒辦法處理太多位元的數字，可能只能拆解 $15=3\times5$ 這種容易的而已。

---

## 單量子位系統 (Single-Qubit Quantum Systems)

在量子計算中，量子位元 (Qubit) 是最基本的資訊單位，類似於傳統計算中的位元 (Bit)。然而，與傳統位元只能處於 0 或 1 的狀態不同，量子位元可以同時處於 0 和 1 的疊加態。先來介紹一下量子計算所處於的空間定義：

### Hilbert 空間 (Hilbert Space)

對於單量子位元系統，Hilbert 空間是一個複數 $\mathbb{C}$ 中的 inner product space，有向量加法、純量乘法，以及計算向量之間的內積。

> If $| \psi \rangle, | \phi \rangle \in V$, $\alpha, \beta \in \mathbb{C}$, then $\alpha | \psi \rangle + \beta | \phi \rangle \in V$.

在單量子位元系統當中，我們常用$|0\rangle=\begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $|1\rangle=\begin{pmatrix} 0 \\ 1 \end{pmatrix}$當作標準基底，而一個量子位元則可表示為 $| \psi \rangle = \alpha | 0 \rangle + \beta | 1 \rangle$。

至於維度 (Dimension) 為 $N$ 的向量空間，則會以 $\{ | e_1 \rangle, | e_2 \rangle, \dots, | e_N \rangle \}$ 當作標準基底，也可以寫成$\{ | 0 \rangle, | 1 \rangle, \dots, | N-1 \rangle \}$ 。

$$|e_1\rangle = \begin{pmatrix} 1 \\ 0 \\ \vdots \\ 0 \end{pmatrix}, |e_2\rangle = \begin{pmatrix} 0 \\ 1 \\ \vdots \\ 0 \end{pmatrix}, \ldots, |e_N\rangle = \begin{pmatrix} 0 \\ \vdots \\ 0 \\ 1 \end{pmatrix}$$

- **正規化 (Normalized)**：$\langle e_i | e_i \rangle = \langle e_2 | e_2 \rangle = \dots = \langle e_N | e_N \rangle = 1$
- **正交 (Orthogoral)**：$\langle e_i | e_j \rangle = \langle e_2 | e_3 \rangle = \dots = \langle e_N | e_N \rangle = 0$

因此：

$$
\langle e_i | e_j \rangle = \delta_{ij} = \begin{cases} 1 & \text{if } i = j \\ 0 & \text{if } i \neq j \end{cases}
$$

**Note**：單量子位元系統的 Hilbert 空間是一個 $N=2$ 的簡單空間，而複數量子位元系統的 Hilbert 空間維度會隨著量子位元數量增加而指數成長，例如 $n=5$ 個量子位元系統的 Hilbert 空間維度為 $N=2^5=32$。

#### 範例

我們拿一個例子來說明好了，假設 $| \psi \rangle = \begin{pmatrix} 1-i \\ 2 \end{pmatrix}$, $| \phi \rangle = \begin{pmatrix} 1 \\ 1+i \end{pmatrix}$，那麼可以做下列這幾個運算：

#### 對偶向量 (Dual Vector)

- $\langle \psi | = \begin{pmatrix} 1-i & 2 \end{pmatrix}^* = \begin{pmatrix} 1+i & 2 \end{pmatrix}$
- $\langle \phi | = \begin{pmatrix} 1 & 1+i \end{pmatrix}^* = \begin{pmatrix} 1 & 1-i \end{pmatrix}$

#### 內積 (Inner Product)

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

- $\langle v | v \rangle \in \mathbb{R}_{\geq 0}$ (non-negative real)
- $\langle v_1 | v_2 \rangle = \overline{\langle v_2 | v_1 \rangle}$
- $(a \langle v_2 | + b \langle v_3 |)| v_1 \rangle = a \langle v_2 | v_1 \rangle + b \langle v_3 | v_1 \rangle$

#### 向量範數 (Norm)

$$
\| \psi \| = \sqrt{\langle \psi | \psi \rangle}
$$

#### 正規化向量 (Normalized Vector)

量子態必須是正規化的，以保證測量結果的機率總和為 1。

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

![Quantum Measurement Single](../../../images/posts/quantum-system/quantum_measurement_single.gif)

#### 範例

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

量子態在測量後會崩塌到某個基底態，而此時的經典態不再具有量子態的疊加性。

### Bloch 球 (Bloch Sphere)

Bloch 球用於表示單量子位的狀態：[video](../../../images/posts/quantum-system/spherical_vs_cartesian_coordinate_systems.mp4)

![Bloch Sphere](../../../images/posts/quantum-system/bloch_sphere.png)

<!-- <div align="center">

<video src="../../../images/posts/quantum-system/spherical_vs_cartesian_coordinate_systems.mp4" width="350" controls autoplay loop muted playsinline>
  Your browser does not support the video tag.
</video>

</div> -->

- $| 0 \rangle \rightarrow (0, 0, 1)$
- $| 1 \rangle \rightarrow (0, 0, -1)$
- $| + \rangle \rightarrow (1, 0, 0)$
- $| - \rangle \rightarrow (-1, 0, 0)$
- $| i \rangle \rightarrow (0, 1, 0)$
- $| -i \rangle \rightarrow (0, -1, 0)$

#### Bloch 球上的 I, X, Y, Z 運算子幾何意義

- **I (單位運算子)**：不改變 Bloch 球上的狀態（即不旋轉）。
- **X 門（Pauli-X）**：繞 $x$ 軸旋轉 $\pi$ 弧度（180°），將 $|0\rangle$ 和 $|1\rangle$ 互換。對應於 Bloch 球上的 $x$ 軸翻轉。
- **Y 門（Pauli-Y）**：繞 $y$ 軸旋轉 $\pi$ 弧度（180°），將 $|0\rangle$ 和 $|1\rangle$ 互換，並帶有相位。對應於 Bloch 球上的 $y$ 軸翻轉。
- **Z 門（Pauli-Z）**：繞 $z$ 軸旋轉 $\pi$ 弧度（180°），將 $|+\rangle$ 和 $|-\rangle$ 互換，$|0\rangle$ 不變，$|1\rangle$ 變號。對應於 Bloch 球上的 $z$ 軸翻轉。

簡單來說，X, Y, Z 分別對應於 Bloch 球上繞 $x$、$y$、$z$ 軸的 180° 旋轉。

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

#### Tensor Product

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

#### 範例

假設：

- 第 0 個 $H_2$：$| 0 \rangle _0 = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $| 1 \rangle _0 = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$, $T_0$ operator
- 第 1 個 $H_2$：$| 0 \rangle _1 = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $| 1 \rangle _1 = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$, $T_1$ operator

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

**Note**：共軛轉置 (conjugate) $U^\dagger = (U^*)^T$

$$
U^{-1} = [\frac{1}{\sqrt{2}} \begin{pmatrix} 1 & i \\ i & 1 \end{pmatrix}]^* = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix}
$$

$$
U^\dagger U = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix} \cdot \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & i \\ i & 1 \end{pmatrix} = \frac{1}{2} \begin{pmatrix} 2 & 0 \\ 0 & 2 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I
$$

$$
U U^\dagger = \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & i \\ i & 1 \end{pmatrix} \cdot \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & -i \\ -i & 1 \end{pmatrix} = \frac{1}{2} \begin{pmatrix} 2 & 0 \\ 0 & 2 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I
$$

$U$ 是 Unitary Matrix

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

Bell State 是兩個 qubit 之間最純粹的糾纏態。

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

![Quantum Measurement Multiple](../../../images/posts/quantum-system/quantum_measurement_multiple.gif)

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

舉個例子：

| $1 \otimes 0 = 1$                                                    | $1 \otimes 1 = 0$                                                    |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| ![Example 1](../../../images/posts/quantum-system/cnot_gate_ex1.png) | ![Example 2](../../../images/posts/quantum-system/cnot_gate_ex2.png) |

這是所有計算中最重要的一個運算門，並且可以延伸出 COPY、NOT 和 SWAP 三種操作：

| COPY                                                                  | NOT                                                                 | SWAP                                                                               |
| --------------------------------------------------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
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

## 量子傳輸

## 量子演算法

### Bernstein-Vazirani Algorithm

### Simon's Algorithm

### Shor's Algorithm

### Grover's Algorithm

## References

- [Shor's Algorithm - 量子計算初學者的理解](https://howardpeng911.medium.com/shor-algorithm-2c1abca22da2)
- [How Quantum Computers Break Encryption | Shor's Algorithm Explained](https://youtu.be/lvTqbM5Dq4Q)
