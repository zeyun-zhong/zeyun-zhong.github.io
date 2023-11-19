---
layout: post
toc:
  sidebar: left

title: Columnspace and Nullspace of a Matrix A and the Solvability of Ax = b
---

## 1. Column Space
The column space denoted by $\boldsymbol{C(A)}$ contains all linear combinations of the columns of matrix $\boldsymbol{A}$ ($m\times n$). It is a subspace of $\mathbb{R}^m$, since the largest dimension it can reach is the dimension of the $\boldsymbol{b}$-vector. We illustrate by a system of $m=3$ equations and $n=2$ unknowns:

$$
\left[
    \begin{matrix}
        1 & 0 \\
        5 & 4 \\
        2 & 4
    \end{matrix} \right]
\left[
    \begin{matrix}
        u \\
        v
    \end{matrix} \right]
=
\left[
    \begin{matrix}
        b_1 \\ b_2 \\b_3
    \end{matrix} \right]. \tag{1}
$$

As we have more equations than unknowns in this example, and this usually means there will be no solution. One example is illustrated in the figure below. Since there is no point which belongs to all of three lines, the system has no solution.

![](/assets/images/Linear Algebra/overdetermined_system.png){:width=30% .align-center}
*Figure 1. Linear System with 3 Equations and 2 Unknows.*

But there is still chance that the system will be solvable for a subset of all possible $\boldsymbol{b}$s. One way to check this is to check if $\boldsymbol{b}$ lies in the column space. We can reformulate the equation (1) by columns:

$$
u\left[
    \begin{matrix}
        1 \\ 5 \\ 2
    \end{matrix} \right]
+
v\left[
    \begin{matrix}
        0 \\ 4 \\ 4
    \end{matrix} \right]
=
\left[
    \begin{matrix}
        b_1 \\ b_2 \\ b_3
    \end{matrix} \right]. \tag{2}
$$

We can describe all combinations of the two columns geometrically: $\boldsymbol{Ax}=\boldsymbol{b}$ can be
solved if and only if $\boldsymbol{b}$ lies in the plane that is spanned by the two column vectors (Figure 2). If $\boldsymbol{b}$ lies off the plane, then it is not a combination of the two columns, corresponding to no solution of the linear system.

![](/assets/images/Linear Algebra/columnspace.png){:width=45% .align-center}
*Figure 2. The column space C(A), a plane in three-dimensional space.*

## 2. Nullspace
The nullspace of a matrix consists of all vectors $\boldsymbol{x}$ such that $\boldsymbol{Ax}=\boldsymbol{0}$. It is denoted by $\boldsymbol{N(A)}$. It is a subspace of $\mathbb{R}^n$, since the $\boldsymbol{x}$-vector is $n$-dimensional. 

## 3. Echelon Form and Row Reduced Form
Let us now review the echelon form und the row reduced echelon form of a matrix $\boldsymbol{A}$. Suppose we have a 3 by 4 matrix:

$$
\boldsymbol{A} = \left[
 \begin{matrix}
   1 & 3 & 3 & 2 \\
   2 & 6 & 9 & 7 \\
   -1 & -3 & 3 & 4
  \end{matrix}
  \right]
$$

After doing [row reduction](https://en.wikipedia.org/wiki/Gaussian_elimination), we get the echelon matrix $\boldsymbol{U}$:

$$
\boldsymbol{U} = \left[
    \begin{matrix}
        \mathbf{1} & 3 & 3 & 2 \\
        0 & 0 & \mathbf{3} & 3 \\
        0 & 0 & 0 & 0
    \end{matrix}
    \right]
$$

The first nonzero entries in their rows are called pivots (in bold typeface). The rows contain pivots are the pivot rows and the columns contain pivot are the pivot columns. In this example, we have two pivot rows and two pivot columns.

We can go further than $\boldsymbol{U}$, to make the matrix even simpler: first make all pivots 1 and then produce zero above the pivots. The final result is the reduced row echelon form $\boldsymbol{R}$.

$$
\left[
    \begin{matrix}
        1 & 3 & 3 & 2 \\
        0 & 0 & 3 & 3 \\
        0 & 0 & 0 & 0
    \end{matrix}
    \right] \rightarrow
\left[
    \begin{matrix}
        1 & 3 & 3 & 2 \\
        0 & 0 & 1 & 1 \\
        0 & 0 & 0 & 0
    \end{matrix}
    \right] \rightarrow
\left[
\begin{matrix}
    1 & 3 & 0 & -1 \\
    0 & 0 & 1 & 1 \\
    0 & 0 & 0 & 0
\end{matrix}
\right] = \boldsymbol{R}.
$$

## 4. Pivot Variables, Free Variables and Rank
The pivots are crucial to read off all the solutions to $\boldsymbol{Rx}=\boldsymbol{0}$.

$$
\boldsymbol{Rx}=
\left[
    \begin{matrix}
        \mathbf{1} & 3 & \mathbf{0} & -1 \\
        \mathbf{0} & 0 & \mathbf{1} & 1 \\
        \mathbf{0} & 0 & \mathbf{0} & 0
    \end{matrix} \right]
\left[
    \begin{matrix}
        u \\ v \\ w \\ y
    \end{matrix} \right]
= \left[
    \begin{matrix}
        0 \\ 0 \\ 0
    \end{matrix} \right] \tag{3}
$$

The unknows $u$, $v$, $w$, $y$ can be divided into two groups: **pivot variables** which correspond to the pivot columns (in boldface), and **free variables**, corresponging to columns without pivots. Accoring to Equation 3, the pivot variables are completely determined in terms of the freee variables $v$ and $y$:

$$
\begin{aligned}
    u + 3v - y &= 0 \quad &\text{yields} \quad u &= -3v + y \\
    w + y &= 0 \quad &\text{yields} \quad w &=  -y
\end{aligned} \tag{4}
$$

If we set $v=1, y=0$ and $v=0, y=1$, then we get two special solutions. The complete solution to $\boldsymbol{Rx}=\mathbf{0}$, or equivalently to $\boldsymbol{Ax}=\mathbf{0}$, is a combination of these two special solutions:

$$
\boldsymbol{x} = 
\left[
    \begin{matrix}
        -3v+y \\ v \\ -y \\ y
    \end{matrix} \right]
=v \left[
    \begin{matrix}
        -3 \\ 1 \\ 0 \\ 0
    \end{matrix} \right]
+ y \left[
    \begin{matrix}
        1 \\ 0 \\ -1 \\ 1
    \end{matrix} \right]. \tag{5}
$$

Each free variable produces its own special solution. The nullspace $\boldsymbol{N(A)}$ is therefore generated by these special solutions and the dimension of the nullspace corresponds to the number of the free variables.

On the other side, the pivot variables count for the column space. The number of pivot variables $r$ corresponds to the number of independent columns (and independent rows) of matrix $\boldsymbol{A}$, which is also called the **rank** denoted by $rank(\boldsymbol{A})$.


## 5. Solving Ax=b, Ux=c, and Rx=d
The case $\boldsymbol{b} \neq \mathbf{0}$ is quite different from $\boldsymbol{b} = \mathbf{0}$. The row operations on $\boldsymbol{A}$ must act also on the right-hand side ($\boldsymbol{b}$). For the original example $\boldsymbol{Ax}=\boldsymbol{b}$, apply to both sides the operations that led from $\boldsymbol{A}$ to $\boldsymbol{U}$. The result is an upper triangular system $\boldsymbol{Ux} = \boldsymbol{c}$:

$$
\left[
    \begin{matrix}
        1 & 3 & 3 & 2 \\
        0 & 0 & 3 & 3 \\
        0 & 0 & 0 & 0
    \end{matrix} \right]
\left[
    \begin{matrix}
        u \\ v \\ w \\ y
    \end{matrix} \right]
= \left[
    \begin{matrix}
        b_1 \\ b_2 - 2b_1 \\ b_3 - 2b_2 + 5b_1
    \end{matrix} \right]. \tag{6}
$$

To make the system solvable, the vector $\boldsymbol{b}$ must lie in the column space generated by the pivot columns (column 1 and 3 in this case). Equivalently, all vectors $\boldsymbol{b}$ in that space must satisfy $b_3 - 2b_2 + 5b_1 = 0$. Here we choose $\boldsymbol{b} = (1,5,5)$ and back-substitution gives:

$$
\begin{aligned}
    3w + 3y &= 3 \quad &\text{or} \quad w &= 1 - y \\
u + 3v + 3w + 2y &= 1 \quad &\text{or} \quad u &= -2 - 3v + y.
\end{aligned} \tag{7}
$$

Setting all free variables ($v$ and $y$) to zero, we get a particular solution $\boldsymbol{x_p} = (-2, 0, 1, 0)$. Since every solution to $\boldsymbol{Ax}=\boldsymbol{b}$ is the sum of one particular solution and a solution to $\boldsymbol{Ax}=\mathbf{0}$:

$$
\boldsymbol{x}_\text{complete} = \boldsymbol{x}_\text{particular} + \boldsymbol{x}_\text{nullspace}.
$$

The complete solution would be:

$$
\boldsymbol{x} = \left[
    \begin{matrix}
        u \\ v\\ w \\ y
    \end{matrix} \right]
= \left[
    \begin{matrix}
        -2 \\ 0 \\ 1 \\ 0
    \end{matrix} \right]
+ v \left[
    \begin{matrix}
        -3 \\ 1 \\ 0 \\ 0
    \end{matrix} \right]
+ y \left[
    \begin{matrix}
        1 \\ 0 \\ -1 \\ 1
    \end{matrix} \right]. \tag{8}
$$

When the equation was $\boldsymbol{Ax}=\mathbf{0}$, the particular solution was the zero vector! The reduced form $\boldsymbol{R}$ makes this solution even clearer. However, this process will not be repeated here.

## 6. Number of Possible Solutions of Ax=b
After introducing the previous sections, we are finally able to conclude the solvability of a linear system $\boldsymbol{Ax}=\mathbf{b}$ and the possible number of solutions.

Still remember the definition of the rank introduced in Section 4.? It is the number of pivot variables and also the maximal number of linearly independent columns (and rows) of matrix $\boldsymbol{A}$. It is crucial for analysis of the solvability of a linear system.

- $r = m = n$: Since the number of free variables ($n-r$) equals zero, there are no free variables, leading to one single solution. The matrix $\boldsymbol{A}$ is invertible.
- $r = n < m$: There are still no free variables, since $n-r = 0$. However, there are zero rows in the echelon form $\boldsymbol{U}$ or in the reduced form $\boldsymbol{R}$, like in Equation 6. This means, there are certain constraints the $\boldsymbol{b}$-vector must satisfy. Thus, there could be one single solution or no solution. Another way to think of it: The dimension of the column space is smaller than the dimension of the $\boldsymbol{b}$-vector. If $\boldsymbol{b}$ lies off the column space, then there is no solution.
- $r = m < n$: There are free variables, corresponding to infinitely many solutions.
- $r < m$, $r < n$: There are free variables and zero rows in $\boldsymbol{U}$ and $\boldsymbol{R}$. So there will be either infinitely many solutions or no solution.

Here is a table to summarize the conclusions above:

|                 | No. of free variables | No. of zero rows in $\boldsymbol{U}$ or $\boldsymbol{R}$| No. of possible solutions|
| -----------     | ----------- | ----------- | ----------- |
| $r = m = n$     | 0           |0            |1            |
| $r = n < m$     | 0           |$m-r$        |0 or 1       |
| $r = m < n$     | $n-r$       |0            |$\infty$     |
| $r < m$, $r < n$| $n-r$       |$m-r$        |0 or $\infty$|

## Reference
- Strang, G. (2006). Linear Algebra and Its Applications, 4th Edition (4th edition). Belmont, CA: Cengage Learning.
- [Lectures on Linear Algebra from Gilbert Strang](https://www.youtube.com/watch?v=9Q1q7s1jTzU&list=PL49CF3715CB9EF31D&index=8&ab_channel=MITOpenCourseWare)
