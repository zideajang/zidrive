
### 求解投影矩阵 M
$$
p = MP_w
$$

$$
M = \begin{bmatrix}
    m_1\\
    m_2\\
    m_3\\
\end{bmatrix}
$$

$$
p_i = \begin{bmatrix}
    u_i\\
    v_i
\end{bmatrix}= \begin{bmatrix}
    \frac{m_1P_i}{m_3P_i}\\
    \frac{m_2P_i}{m_3P_i}
\end{bmatrix}
$$

一对点可以得到 2 方程，所以最少 6 对点，为了避免噪声，通常会取多余 6 对点来计算

$$
u_i = \frac{m_1P_i}{m_3P_i}\\
u_i(m_3P_i) = m_1P_i\\
m_1P_i -  u_i(m_3P_i) = 0\\
$$

$$
v_i = \frac{m_2P_i}{m_3P_i}\\
v_i(m_3P_i) = m_2P_i\\
m_2P_i -v_i(m_3P_i)= 0
$$

### 标定问题


$$
-u_1(m_3P_1) + m_1P_1  = 0\\
-v_1(m_3P_1) + m_2P_1 = 0\\
\vdots\\
-u_n(m_3P_n) + m_1P_n  = 0\\
-v_n(m_3P_n) + m_2P_n = 0
$$

$$
P = \begin{pmatrix}
    P_1^T & 0^T & -u_1P_1^T\\
    0^T  & P_1^T & -v_1P_1^T\\
    \vdots & \vdots & \vdots\\
    P_n^T & 0^T & -u_nP_n^T\\
    0^T  & P_n^T & -v_nP_n^T\\
\end{pmatrix}_{2n \times 12}
$$

$$
m = \begin{pmatrix}
    m_1^T\\
    m_2^T\\
    m_3^T\\
\end{pmatrix}_{12 \times 1}
$$

### 齐次线性方程组的最小二乘解

$$
x^{*} = \argmin_x ||Ax||\\
s.t. ||x|| =1
$$
- $A = UDV^T$
- $x^*$ 为 V 矩阵的最后一列（最小值对应的右特征向量）

$$
U_{2n\times 12} D_{12 \times 12} V^T_{12 \times 12}
$$

$$
m = \begin{pmatrix}
    m_1^T\\
    m_2^T\\
    m_3^T\\
\end{pmatrix} 
$$

$$
M  = \begin{bmatrix}
    m_1\\
    m_2\\
    m_3\\
\end{bmatrix} = \begin{bmatrix}A & b\end{bmatrix}
$$

### 计算摄像机的内参和外参

$$
p^{\prime} = MP = K \begin{bmatrix}R & T\end{bmatrix}P
$$

$$
K =  \begin{bmatrix}
\alpha & - \alpha \cot \theta & u_0\\  
0 & \frac{\beta}{\sin \theta} & v_0\\
0 & 0 & 1  
\end{bmatrix}
$$

$$
R =  \begin{bmatrix}
r_1^T\\
r_2^T\\
r_3^T\\
\end{bmatrix} T =  \begin{bmatrix}
t_x\\
t_y\\
t_z\\
\end{bmatrix} 
$$

$$
r_1^T =\begin{bmatrix} r_{11} & r_{12} & r_{13}\end{bmatrix} 
$$

$$
\begin{bmatrix}
r_{11} & r_{12} & r_{13} & t_x\\
r_{21} & r_{22} & r_{23} & t_y\\
r_{31} & r_{32} & r_{33} & t_z\\
\end{bmatrix} 
$$

$$
\begin{bmatrix}
\alpha r_{11} - \alpha \cot \theta  r_{21} + u_0 r_{33} & 
\alpha r_{12} - \alpha \cot \theta  r_{22} + u_0 r_{32} &
\alpha r_{13} - \alpha \cot \theta  r_{23} + u_0 r_{33}
 & \alpha t_x - \alpha \cot \theta  t_y + u_0 t_z
 \end{bmatrix}
$$

$$
\alpha r_{11} - \alpha \cot \theta  r_{21} + u_0 r_{33} \\
\alpha r_{12} - \alpha \cot \theta  r_{22} + u_0 r_{32} \\
\alpha r_{13} - \alpha \cot \theta  r_{23} + u_0 r_{33}
$$


### 计算摄像机外参数 T
$$
\rho \begin{bmatrix}
    A & b
\end{bmatrix} = K \begin{bmatrix}
    R & T
\end{bmatrix}
$$

$$
A =  \begin{bmatrix}
    a_1^T\\
    a_2^T\\
    a_3^T\\
\end{bmatrix} b = \begin{bmatrix}
    b_1\\
    b_2\\
    b_3\\
\end{bmatrix} 
$$

$$
T = \rho K^{-1}b
$$

### 计算摄像机参数

$$
\rho = \frac{1}{\begin{vmatrix}
    a_3
\end{vmatrix}}
$$

$$
u_0 = \rho^2(a_1 \dot a_3)
v_0 = \rho^2(a_2 \dot a_3)
$$

$$
\cos \theta - \frac{(a_1 \times a_3)\dot(a_2 \times a_3)}{\begin{vmatrix}
    a_1 \times a_3
\end{vmatrix} \cdot \begin{vmatrix}
    a_2 \times a_3
\end{vmatrix}}
$$

$$
\alpha = \rho^2 \begin{vmatrix}
    a_1 \times a_3
\end{vmatrix}\sin \theta
$$


$$
\beta = \rho^2 \begin{vmatrix}
    a_2 \times a_3
\end{vmatrix}\sin \theta
$$

#### 外参数
$$
r_1 = \frac{(a_2 \times a_3)}{ \begin{vmatrix}
    a_2 \times a_3
\end{vmatrix}}
$$

$$
r_3 = \frac{a_3}{ \begin{vmatrix}
    a_3
\end{vmatrix}}
$$

$$
r_2 = r_3 \times r_1
$$