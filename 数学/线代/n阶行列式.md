
$n$ 阶行列式是取自==不同行不同列==的 $n$ 个元素乘积的代数和，即
$$
D=
\begin{vmatrix}
a_{11} & a_{12} & \cdots & a_{1n}\\
a_{21} & a_{22} & \cdots & a_{2n}\\
\vdots & \vdots & & \vdots\\
a_{n1} & a_{n2} & \cdots & a_{nn}
\end{vmatrix}
=\sum_{j_1j_2\cdots j_n}(-1)^{\tau(j_1j_2\cdots j_n)}a_{1j_1}a_{2j_2}\cdots a_{nj_n}.
$$
特别地，一阶行列式 $|a|=a$。

### 几种特殊行列式

（1）对角行列式、上（下）三角行列式的值等于主对角线元素之积
$$
D=
\begin{vmatrix}
a_{11} & & &\\
& a_{22} & &\\
& & \ddots &\\
& & & a_{nn}
\end{vmatrix}
=
\begin{vmatrix}
a_{11} & a_{12} & \cdots & a_{1n}\\
0 & a_{22} & \cdots & a_{2n}\\
\vdots & \vdots & & \vdots\\
0 & 0 & \cdots & a_{nn}
\end{vmatrix}
=
\begin{vmatrix}
a_{11} & 0 & \cdots & 0\\
a_{21} & a_{22} & \cdots & 0\\
\vdots & \vdots & & \vdots\\
a_{n1} & a_{n2} & \cdots & a_{nn}
\end{vmatrix}
=a_{11}a_{22}\cdots a_{nn}.
$$
（2）副对角线行列式
$$
D=
\begin{vmatrix}
& & & a_{1n}\\
& & a_{2,n-1} &\\
& \cdots & &\\
a_{n1} & & &
\end{vmatrix}
=
\begin{vmatrix}
0 & \cdots & 0 & a_{1n}\\
0 & \cdots & a_{2,n-1} & a_{2n}\\
\vdots & & \vdots & \vdots\\
a_{n1} & \cdots & a_{n,n-1} & a_{nn}
\end{vmatrix}
=
\begin{vmatrix}
a_{11} & \cdots & a_{1,n-1} & a_{1n}\\
a_{21} & \cdots & a_{2,n-1} & 0\\
\vdots & & \vdots & \vdots\\
a_{n1} & \cdots & 0 & 0
\end{vmatrix}
=(-1)^{\frac{n(n-1)}{2}}a_{1n}a_{2,n-1}\cdots a_{n1}.
$$


（3）范德蒙行列式
$$
\begin{vmatrix}
1 & 1 & \cdots & 1\\
a_1 & a_2 & \cdots & a_n\\
\vdots & \vdots & & \vdots\\
a_1^{n-1} & a_2^{n-1} & \cdots & a_n^{n-1}
\end{vmatrix}
=\prod_{1\le j<i\le n}(a_i-a_j).
$$
