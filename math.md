# 控制系統傳遞函數推導與分析報告

## 一、 問題描述
某電樞控制式直流電機，用於機器人關節轉速控制，已知參數如下：
* **電樞回路電阻**： $R_a = 1\,\Omega$
* **電樞電感**： $L_a = 0.1\,\text{H}$
* **電機轉動慣量**： $J = 0.01\,\text{kg}\cdot\text{m}^2$
* **阻尼係數**： $B = 0.1\,\text{N}\cdot\text{m}\cdot\text{s/rad}$
* **反電動勢常數**： $K_e = 0.05\,\text{V}\cdot\text{s/rad}$
* **轉矩常數**： $K_t = 0.05\,\text{N}\cdot\text{m/A}$

**系統輸入**為電樞電壓 $u_a(t)$，**系統輸出**為電機角速度 $\omega(t)$。初始狀態為靜止（所有初始條件為 0）。

**目標**：
1. 通過拉普拉斯變換（Laplace Transform）推導從輸入 $u_a(t)$ 到輸出 $\omega(t)$ 的傳遞函數 $G(s) = \frac{\Omega(s)}{U_a(s)}$。
2. 分析角速度對單位階躍輸入（Unit Step Input）的回應。

---

## 二、 數學模型建立
根據直流電機的物理特性，系統可以分為**電氣迴路**與**機械轉動**兩個部分：

### 1. 電氣迴路方程（基爾霍夫電壓定律 KVL）
電樞電壓等於電阻壓降、電感壓降與馬達旋轉產生的反電動勢 $e_b(t)$ 之和：
$$u_a(t) = R_a i_a(t) + L_a \frac{di_a(t)}{dt} + e_b(t)$$

其中，反電動勢與馬達角速度成正比：
$$e_b(t) = K_e \cdot \omega(t)$$

將其代入得：
$$u_a(t) = R_a i_a(t) + L_a \frac{di_a(t)}{dt} + K_e \omega(t) \quad \text{--- (式 1)}$$

### 2. 機械轉動方程（牛頓第二運動定律）
電機產生的電磁轉矩 $T_m(t)$ 用於克服旋轉慣量以及阻尼轉矩：
$$T_m(t) = J \frac{d\omega(t)}{dt} + B \omega(t)$$

其中，電磁轉矩與電樞電流成正比：
$$T_m(t) = K_t \cdot i_a(t)$$

將其代入得：
$$K_t i_a(t) = J \frac{d\omega(t)}{dt} + B \omega(t) \quad \text{--- (式 2)}$$

---

## 三、 傳遞函數推導
在初始條件為零的假設下，對（式 1）與（式 2）進行拉普拉斯變換：

由（式 1）變換得：
$$U_a(s) = (R_a + L_a s) I_a(s) + K_e \Omega(s) \quad \text{--- (式 3)}$$

由（式 2）變換得：
$$K_t I_a(s) = (J s + B) \Omega(s) \implies I_a(s) = \frac{J s + B}{K_t} \Omega(s) \quad \text{--- (式 4)}$$

將（式 4）代入（式 3）中以消去電流項 $I_a(s)$：
$$U_a(s) = (R_a + L_a s) \cdot \frac{J s + B}{K_t} \Omega(s) + K_e \Omega(s)$$

兩邊同乘以 $K_t$：
$$K_t U_a(s) = \left[ (L_a s + R_a)(J s + B) + K_t K_e \right] \Omega(s)$$

因此，系統的傳遞函數 $G(s)$ 為：
$$G(s) = \frac{\Omega(s)}{U_a(s)} = \frac{K_t}{L_a J s^2 + (R_a J + L_a B)s + (R_a B + K_t K_e)}$$

---

## 四、 代入參數數值
將題目中給定之已知參數值代入傳遞函數：
* $L_a J = 0.1 \times 0.01 = 0.001$
* $R_a J + L_a B = 1 \times 0.01 + 0.1 \times 0.1 = 0.01 + 0.01 = 0.02$
* $R_a B + K_t K_e = 1 \times 0.1 + 0.05 \times 0.05 = 0.1 + 0.0025 = 0.1025$
* $K_t = 0.05$

代入後得到具體的傳遞函數：
$$G(s) = \frac{0.05}{0.001s^2 + 0.02s + 0.1025}$$

為了標準化分子與分母，將分子分母同乘以 $1000$：
$$G(s) = \frac{50}{s^2 + 20s + 102.5}$$

---

## 五、 階躍回應分析
將得到的傳遞函數對照標準二階系統形式進行分析：
$$G(s) = \frac{K \cdot \omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$$

### 1. 系統特徵參數計算
* **固有頻率（無阻尼自然頻率） $\omega_n$**：
  $$\omega_n^2 = 102.5 \implies \omega_n = \sqrt{102.5} \approx 10.124\,\text{rad/s}$$

* **阻尼比 $\zeta$**：
  $$2\zeta\omega_n = 20 \implies \zeta = \frac{20}{2 \times 10.124} \approx \frac{10}{10.124} \approx 0.988$$

* **系統增益倍數 $K$**（非直流增益）：
  $$K \cdot \omega_n^2 = 50 \implies K = \frac{50}{102.5} \approx 0.488$$

### 2. 回應特性討論
* **阻尼狀態**：因為 $0 < \zeta < 1$（且非常接近 1，$\zeta \approx 0.988$），此系統屬於**欠阻尼系統（Underdamped System）**。
* **波形特徵**：由於阻尼比極其接近 1，系統對階躍輸入的回應雖然會有極其微小的超調量（Overshoot），但基本上非常平緩，接近於臨界阻尼。系統不會出現劇烈的震盪，能夠快速且平穩地趨於穩定。
* **穩態值（Steady-state value）**：
  若輸入為單位階躍信號 $u_a(t) = 1(t) \implies U_a(s) = \frac{1}{s}$，根據終值定理（Final Value Theorem）：
  $$\omega_{ss} = \lim_{s \to 0} s \cdot \Omega(s) = \lim_{s \to 0} s \cdot G(s) \cdot \frac{1}{s} = G(0) = \frac{50}{102.5} \approx 0.488\,\text{rad/s}$$
  這意味著，當輸入 $1\,\text{V}$ 的恆定電壓時，馬達最終的穩定角速度將趨近於 $0.488\,\text{rad/s}$。
