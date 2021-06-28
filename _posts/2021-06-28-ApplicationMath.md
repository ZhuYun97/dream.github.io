## Lecture 1
#### 1. BVP(boundary variables problems)
- one dimension
- two dimensions

the matrix is sparse, how to solve this problem

#### 2. Condition numbers
$\kappa(A)=\|A\| \cdot\left\|A^{-1}\right\|$

##### 2.1 matrix norm
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

##### 2.2 stability of linear system
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
##### 2.3 how to calculate condition number