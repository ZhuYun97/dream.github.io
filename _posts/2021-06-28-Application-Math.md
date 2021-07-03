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
full $Q =[Q_1, Q_2]$, how to find $Q_2$ is a main problem, first modify 
$A$ with $[A, I]$, then process it as reduced QR.
$R = $$
\left[\begin{array}{c}
R_{1} \\
0
\end{array}\right]
$

- **QR decomposition by Householder transformation**
    - In Gram-Schmidt orthogonalization:
        - $A \underbrace{R_{1} R_{2} \cdots R_{n}}_{R^{-1}}=Q$
    - Householder triangularization
        - a series of elmentary orthogonal matrices $Q_k$ is applied to the left of $A$, so that the resulting matrix:
        $\underbrace{Q_{n} \cdots Q_{2} Q_{1}}_{O^{\top}} A=R$, The product $Q=Q_{1}^{\top} Q_{2}^{\top} \cdots Q_{n}^{\top}$ is orthogonal
    - The difference betweed Gram-Schmidt and Householder: Gram-schmidt is triangular orthogonalization, but Householder is orthogonal triangularization.

    - The matrix $Q_k$ is chosen to introduce zeros below the diagonal in the k-th column while preserving all the zeros previously introduced.
    - ![hd](/assets/img/am/hd.PNG)
   
    - Householder transformation
        - Definition: $\mathbf{H}=\mathbf{I}-2 \mathbf{v} \mathbf{v}^{\top}$, where $\|\mathbf{v}\|_{2}=1$
        - Proposition:
            1. $\mathbf{H}=\mathbf{H}^{\top} \text { and } \mathbf{H}^{\top} \mathbf{H}=\mathbf{H}^{2}=\mathbf{I}$
            2. $\|\mathbf{H x}\|_{2}=\|\mathbf{x}\|_{2}$
            3. $\mathbf{H x}$ is a reflection of $\mathbf{x}$ with respect to $\operatorname{Span}\{\mathbf{v}\}^{\perp}$. That is, let $\mathbf{x}=\mathbf{x}_{1}+\mathbf{x}_{2}, \mathbf{x}_{1} \in \operatorname{Span}\{\mathbf{v}\}$ and $\mathbf{x}_{2} \in \operatorname{Span}\{\mathbf{v}\}^{\perp}$, then
$\mathbf{H x}=\mathbf{x}_{1}-\mathbf{x}_{2}$
        - Illustration:
            - ![hd2](/assets/img/am/hd2.PNG)
            - The introduce zeros into k-th column($\mathbf{x} \in \mathbb{R}^{m-k+1}$), the Householder transformation $F$ should:
         $$
\mathbf{x}=\left[\begin{array}{c}
\times \\
\times \\
\vdots \\
\times
\end{array}\right] \stackrel{F}{\longrightarrow} F \mathbf{x}=\left[\begin{array}{c}
\|\mathbf{x}\| \\
0 \\
\vdots \\
0
\end{array}\right]=\|\mathbf{x}\| \mathbf{e}_{1}=\alpha \mathbf{e}_{1}
$$
            - Every point $x \in \mathbb{R}^{m}$ is mapped to a mirror point: $F \mathbf{x}=\left(I-2 \frac{\mathbf{u u}^{\top}}{\mathbf{u}^{\top} \mathbf{u}}\right) \mathbf{x}=\mathbf{x}-2 \mathbf{u}\left(\frac{\mathbf{u}^{\top} \mathbf{x}}{\mathbf{u}^{\top} \mathbf{u}}\right)$ and hence $F=\left(I-2 \frac{\mathbf{u u}^{\top}}{\mathbf{u}^{\top} \mathbf{u}}\right)$
        - The better of two Householder reflectors:
            - ![hd3](/assets/img/am/hd3.PNG)
        - For numerical stability pick the one that moves reflect $x$ to the vector $\|\mathbf{x}\| \mathbf{e}_{1}$ that is not to close to $\mathbf{x}$ itself, i.e., $-\|\mathbf{x}\| \mathbf{e}_{1}-\mathbf{x}$ in this case
        - In other words, the better of the two reflectors is:
            - $\mathbf{u}=\operatorname{sign}\left(x_{1}\right)\|\mathbf{x}\| \mathbf{e}_{1}+\mathbf{x}$, where $x_{1}$ is the first element of $x\left(\operatorname{sign}\left(x_{1}\right)=1\right.$ if $\left.x_{1}=0\right)$
    - The processes of Householder QR factorization:
        - Algorithm:
        -   ![hd4](/assets/img/am/hd4.PNG)
    - Example
    ![hd5](/assets/img/am/hd5.PNG)
    ![hd6](/assets/img/am/hd6.PNG)


- **QR decomposition by Given rotation**
Why do we need given rotation?
If we use full QR to solve linear system $Ax = b$, we need to decompress $A$ into $QR$, it needs much time and memory. 
$$
\mathbf{Q}\left[\begin{array}{c}
\mathbf{R} \\
\mathbf{0}
\end{array}\right] \mathbf{x}=\mathbf{b}
$$
Both sides times $Q^T$
$$
\begin{gathered}
\mathbf{Q}^{T} \mathbf{Q}\left[\begin{array}{c}
\mathbf{R} \\
\mathbf{0}
\end{array}\right] \mathbf{x}=\mathbf{Q}^{T} \mathbf{b} \\
{\left[\begin{array}{c}
\mathbf{R} \\
\mathbf{0}
\end{array}\right] \mathbf{x}=\mathbf{Q}^{T} \mathbf{b}} \\
{\left[\begin{array}{c}
\mathbf{R} \\
\mathbf{0}
\end{array}\right] \mathbf{x}=\left[\begin{array}{l}
\mathbf{d} \\
\mathbf{c}
\end{array}\right]}
\end{gathered}
$$

$$
\mathbf{R} \mathbf{x}=\mathbf{d}(3)
$$
Equipped with given rotation, we don't need to calculate $Q$  
To solve $Ax = b$, we multiple  givens matrix on both sides to get the same format above:
$$
\begin{aligned}
&{\left[\begin{array}{cc}
\cos \theta & -\sin \theta \\
\sin \theta & \cos \theta
\end{array}\right] \mathbf{A} \mathbf{x}=\left[\begin{array}{cc}
\cos \theta & -\sin \theta \\
\sin \theta & \cos \theta
\end{array}\right] \mathbf{b}} \\
&{\left[\begin{array}{cc}
b_{11} & b_{12} \\
0 & b_{22}
\end{array}\right]\left[\begin{array}{l}
x_{1} \\
x_{2}
\end{array}\right]=\left[\begin{array}{l}
d_{1} \\
d_{2}
\end{array}\right]}
\end{aligned}
$$
The main processes of Given rotation:
1. From left to right deal with it columnwise
2. For each column, we should process it from bottom to top.
3. For given matrix, it is defined by the element(need to be 0) and digonal element.




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

Full QR factorization
$$
A=\left[\begin{array}{ll}
Q_{1} & Q_{2}
\end{array}\right]\left[\begin{array}{c}
R_{1} \\
0
\end{array}\right]
$$
Multiplication by orthogonal matrix
$$
\begin{aligned}
\|A \mathbf{x}-\mathbf{y}\|^{2} &=\left\|\left[\begin{array}{ll}
Q_{1} & Q_{2}
\end{array}\right]\left[\begin{array}{c}
R_{1} \\
0
\end{array}\right] \mathbf{x}-\mathbf{y}\right\|^{2} \\
&=\left\|\left[\begin{array}{ll}
Q_{1} & Q_{2}
\end{array}\right]^{\top}\left[\begin{array}{ll}
Q_{1} & Q_{2}
\end{array}\right]\left[\begin{array}{c}
R_{1} \\
0
\end{array}\right] \mathbf{x}-\left[\begin{array}{ll}
Q_{1} & Q_{2}
\end{array}\right]^{\top} \mathbf{y}\right\|^{2} \\
&=\left\|\left[\begin{array}{c}
R_{1} \mathbf{x}-Q_{1}^{\top} \mathbf{y} \\
-Q_{2}^{\top} \mathbf{y}
\end{array}\right]\right\|^{2} \\
&=\left\|R_{1} \mathbf{x}-Q_{1}^{\top} \mathbf{y}\right\|^{2}+\left\|Q_{2}^{\top} \mathbf{y}\right\|^{2}
\end{aligned}
$$
Least  squares minimization:
$$
\|A \mathbf{x}-\mathbf{y}\|^{2}=\left\|R_{1} \mathbf{x}-Q_{1}^{\top} \mathbf{y}\right\|^{2}+\left\|Q_{2}^{\top} \mathbf{y}\right\|^{2}
$$

Opimtal solution:
$$
\mathbf{x}_{l s}=R_{1}^{-1} Q_{1}^{\top} \mathbf{y}
$$
Residual of the optimal is:
$$
A \mathbf{x}_{ls}-\mathbf{y}=-Q_{2} Q_{2}^{\top} \mathbf{y}
$$

### 5. Error Analysis
ill-conditioned problem: if small variations in the data lead to very large variation in the solution.

#### 5.1 Perturbation Analysis
$A$ into $A(\varepsilon)=A+\varepsilon E$ and $b$ into $b+\varepsilon e$. The solution $x(\varepsilon)$ of perturbed system is such that:
$(A+\varepsilon E) x(\varepsilon)=b+\varepsilon e$
Let $\delta(\varepsilon)=x(\varepsilon)-x$:
$$(A + \varepsilon E)\delta(\varepsilon)= \varepsilon (e-Ex)$$
$$\delta(\varepsilon)=\varepsilon (A+\varepsilon E)^{-1}(e-Ex)$$
$x(\varepsilon)$ is differentiable at 0 and its derivative is:
$$
x^{\prime}(0)=\lim _{\varepsilon \rightarrow 0} \frac{\delta(\varepsilon)}{\varepsilon}=A^{-1}(e-E x)
$$
A small variation $[\varepsilon E, \varepsilon e]$ will cause the solution to vary by roughly $\varepsilon X^{\prime}(0)=\varepsilon A^{-1}(e-E x)$
The relative variation is such that:



### 6. Iterative Method

#### 6.1 Basic concepts

- Residual and Error
    - error $e = u-v$, error indicates how far we are from the solution
    - residual $r = f- Av$, $Au=f$ as $A(v+e)=f$ which means $Ae = f- Av$, so the residual can be writen as $Ae = r$, residual 
    - residual correction: $u=v+e$

#### 6.2 Steepest Descent
A quadratic function of a vector with the form:
$$
f(x)=\frac{1}{2} x^{T} A x-b^{T} x+c
$$

The gradient of the quadratic form is given by(if A is symmetric)
$$
\nabla f(x)=\frac{1}{2} A^{T} x+\frac{1}{2} A x-b=A x-b
$$
Therefore, if $x$ is solution of the linear system $Ax=b$, then $\nabla f(x)$ is equal to  0 and x is a critical point of $f(x)$. If $A$ is positive definite, as well as symmetric, then this critical point is a minimun of $f(x)$. So $Ax=b$ can be solved by finding an $x$ that minimizes $f(x)$.
#### 3.1 Gaussian Elimination / the Cholesky decomposition
Gaussian elimination with pivoting is equivalent to LU decomposition:
$$
\Pi A=L \cdot U
$$
$\Pi$ is a permutation matrix, L and U are lower and upper triangular.


#### 3.2 Iterative solution


### Tensors


