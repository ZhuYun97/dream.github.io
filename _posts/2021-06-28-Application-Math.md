## Lecture 1 Introduction and matrix analysis
### 1. BVP(boundary variables problems)
- one dimension
- two dimensions

the matrix is sparse, how to solve this problem

### 2. Condition numbers
$\kappa(A)=\|A\| \cdot\left\|A^{-1}\right\|$

#### 2.1 matrix norm
In order to understand condition numbers easily, we should learn some extra knowledges, like matrix norm

vector is a point in the space, vector norm is the length of the vector.
as for matrix, it is a transformation for a point, vector is an entity, but the matrix is a transformation for an entity, the length of an entity is a norm. the extent of transformation of an entity can also be called norm. The matrix norm is bigger, the extent of transformation of a vector can be bigger.

$$
\begin{equation}
\|A\|=\max _{x} \frac{\|A x\|}{\|x\|}
\end{equation}
$$

$$
\begin{equation}
\left\|A^{-1}\right\|=\max _{y} \frac{\left\|A^{-1} y\right\|}{\|y\|}=1 / \min _{y} \frac{\|y\|}{\left\|A^{-1} y\right\|}=1 / \min _{x} \frac{\|A x\|}{\|x\|}
\end{equation}
$$

in the three equation, we use $Ay=x$
the stretching power of $A$ is the same as the squeezing power of $A^{-1}$, we combine both powers to get condition numbers 

$$
\begin{equation}
\kappa(A)=\|A\| \cdot\left\|A^{-1}\right\|
\end{equation}
$$

#### 2.2 stability of linear system
given a linear equation $Ax=b$, how to evaluate the stability of the equation. $A$ is parameter matrix which is defined by model, $b$ is observation value. If small permutation of $b$ will not make the solution $x$ change rapidly. It can be called as stable equation. In other words, stability measures the ability of a linear equation to resist noise.

If we add noise $\delta b$ to observation variable, the solution will be $x + \delta x$
$$
A(x+\delta x)=b+\delta b 
$$

$$
A \delta x=\delta b
$$

to calculate norm of both sides
$$
\|A \delta x\|=\|\delta b\|
$$

according to (1), we can get $
\|A\| \geq \frac{\|A \delta x\|}{\|\delta x\|}
$, combining with (3)

$$
\begin{equation}
\|A\|\|\delta x\| \geq\|A \delta x\|=\|\delta b\|
\end{equation}
$$

accordint to (2) $
\left\|A^{-1}\right\| \geq 1 / \frac{\|A  x\|}{\| x\|}
$, combining with $\|Ax\| = \|b\|$

$$
\begin{equation}
\|A^{-1} \| \|b \| = \|x \|
\end{equation}
$$

Multiply the left and right sides of (4) and (5) to get:
$$
\frac{1}{\kappa(A)} \frac{\|\delta b\|}{\|b\|} \leq \frac{\|\delta x\|}{\|x\|}
$$

the lower bound of the variation of x is influenced by the variation of b. For instance, condition number is 10, the variation of b is 100%, then the variation of  x is at least 10%.
The up bound can be calculated in the similar way,

$$
\frac{\|\delta x\|}{\|x\|} \leq \kappa(A) \frac{\|\delta b\|}{\|b\|}
$$

Combining upper and lower bound,
$$
\begin{equation}
\frac{1}{\kappa(A)} \frac{\|\delta b\|}{\|b\|} \leq \frac{\|\delta x\|}{\|x\|} \leq \kappa(A) \frac{\|\delta b\|}{\|b\|}
\end{equation}
$$
We can conclude that the condition number determine how much the solution $x$ is affected by the noise of the observation value. The bigger condition number is, the less stable $x$ is. 
#### 2.3 how to calculate condition number
the biggest singular value divided by smallest singular value
$$
\begin{equation}
\kappa(A)=\sigma_{\max } / \sigma_{\min }
\end{equation}
$$

### 3. Matrix Decomposition
- **LU decomposition**: $A = LU$, $A$ is square matrix
- Cholesky decomposition: $A = U^{T}U$, U is upper triangular with positive diagonal entries. applicable to square, symmetric, positive definite matrix A
- **QR decomposition**: Q is a m-by-m orthogonal matrix, R is an m-by-n upper triangular matrix
- Eigendecomposition
- Schur decomposition
- **Singular value decomposition**: $A=U \Sigma V^{\top}$

#### 3.1 LU decomposition
$$\mathbf{A}=\mathbf{L U}$$
where $\mathbf{L} \in \mathbb{R}^{n \times n}$ is lower triangular and $\mathbf{U} \in \mathbb{R}^{n \times n}$ is upper triangular

LU decomposition doesn't always exist, sometimes we need an extra permutation matrix:
$$
\mathbf{P A}=\mathbf{L} \mathbf{U}
$$

---

Further reading
- if $\mathbf{A} \in \mathbb{S}^{n \times n}$ has an LU decomposition, then $\mathbf{U}=\mathbf{D L}^{T}$ where D is diagonal
- Chlesky factorization: if $\mathbf{A} \in \mathbb{S}^{n \times n}$ is PD, it can always be factored as $\mathbf{A}=\mathbf{G} \mathbf{G}^{T}$, G is lower triangular
- LDM: another way of writing LU, $\mathbf{A}=\mathbf{L} \mathbf{D} \mathbf{M}^{T}$, where $\mathbf{M}=\mathbf{U}^{T} \mathbf{D}^{-1}$
- LDL: if A is symmetric, LDM can be reduced to $\mathbf{A}=\mathbf{L} \mathbf{D} \mathbf{L}^{T}$

#### 3.2 QR decomposition
- **Orthonormal basis**
Let the columns of $U = [u_1, u_2, ..., u_n]$ are orthonormal, $U^{T}U=I$
Multiplication by $U$ perserves inner products, angles, and distances
- **Gram-Schmidt process**
![gs](/assets/img/am/gs.PNG)

then $R = Q^{T}A$

- Example(Reduced QR)
![gs](/assets/img/am/gs2.PNG)
![gs](/assets/img/am/gs3.PNG)

- The difference between full QR anf reduced QR
    - The reduced QR only satisfy $Q^TQ=I$ but not sastify $QQ^T=I$
![gs](/assets/img/am/qr.PNG)

> How to find full Q
$Q =[Q_1, Q_2]$



### 4. Solving Linear Systems
Problem: compute the solution to $\mathbf{A x}=\mathbf{b}$ in a numerically efficient manner

- the problem is easy if $A^{-1}$ is known
    - but computing $A^{-1}$ costs computations
    - how to compute $A^{-1}$ efficiently
- A is assumed to be a general nonsingular matrix

#### 4.1 LU to solve linear systems
Solving $\mathbf{A x}=\mathbf{b}$ can be recast as two linear system problems:
1. solve $\mathbf{L z}=\mathbf{b}$ for $\mathbf{z}$, and then
2. solve $\mathbf{U x}=\mathbf{z}$ for $\mathbf{x}$

#### 4.2 direct solving method for least square method
Solving $\mathbf{A x}=\mathbf{b}$
When matrix $\mathbf{A}$ is overdetermined, $\mathbf{b}$ are not in the $span\{\mathbf{A}\}$. In other words, this equation doesn't have exact solution. But we can get a approximate solution $x_{ls}$ which:
$$
\min _{x}\|b-A x\|_{2}
$$
We can use gradient descent to get optimal value, which corresponds to $\mathbf{A x}-\mathbf{b}$ is vertical to $span\{\mathbf{A}\}$ from geometric perspective.
The residual vector $mathbf{r} = \mathbf{b} - \mathbf{A x}$ is vertical to span{A}, so it can be formulated as:
$$
r^{T}\left(\begin{array}{llll}
a_{\cdot 1} & a_{\cdot 2} & \cdots & a_{\cdot n}
\end{array}\right)=r^{T} A=0
$$

$$
A^{T} A x=A^{T} b
$$
If the column vectors are independent, the solution have unique solution:
$$
X=\left(A^{T} A\right)^{-1} A^{T} B
$$

This method is quite simple, but it has two main weaknesses:
1. the matrix multiplication will lead to cut-off. Because of cut-off, the matrix $AA^{T}$ will become a singular matrix which doesn't have inverse matrix.
2. The condition number of $AA^{T}$ is square times than $A$. The system becomes less stable. We can centeralize data in order to add orthogonality of basises.

#### 4.3 QR to solve linear system
Both reduced QR and full QR can solve linear system:
$$
\hat{R} x=\hat{Q}^{T} b
$$

####

#### 3.1 Gaussian Elimination / the Cholesky decomposition
Gaussian elimination with pivoting is equivalent to LU decomposition:
$$
\Pi A=L \cdot U
$$
$\Pi$ is a permutation matrix, L and U are lower and upper triangular.


#### 3.2 Iterative solution

## Lecture 2 Iterative Method to Linear equation System

