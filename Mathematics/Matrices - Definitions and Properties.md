#math #linear_algebra 

# Definitions and Properties

## A grid of numbers

```{index} Matrices; definition
```

The concept of a **matrix** extends the one-dimensional (i.e. single row/column) idea of a vector into two indices (i.e. both rows and columns). A matrix of a given shape can be written as a rectangular array of numbers,

$$ \mathrm{M} =
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}
$$

Where here the matrix has three rows and three columns. We can refer to the individual elements of the matrix by their row and column indices, i.e. $M_{ij}$ is the element in the $i$-th row and $j$-th column of the matrix $\mathrm{M}$ and in this case $M_{12}= b$. The number of rows and columns of a matrix are referred to as its _dimensions_. The elements could be any type of number, but for most of your programme, we will only be using **real numbers**.

We will define the _action_ of a matrix on a column vector to be the matrix-vector product, where we take the dot product of each row of matrix in turn with the column vector,

$$ \mathrm{M}\mathbf{v} =
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z
\end{bmatrix}
= \begin{bmatrix}
ax + by + cz \\
dx + ey + fz \\
gx + hy + iz
\end{bmatrix}
$$

This can be written in a more compact form using the "Sigma" (summation) notation as

$$ [\mathrm{M}\mathbf{v}]_i = \sum_{j=1}^n M_{ij}v_j $$

where $[\mathrm{M}\mathbf{v}]_i$ is the $i$-th component of the resulting vector, and $M_{ij}$ is the $i,j$-th element of the matrix $\mathrm{M}$.
The matrix-vector product is only well defined if the number of columns of the matrix equals the number of rows of the vector. Such matrices are said to be _conformable_ for multiplication. The resulting vector has the same number of rows as the matrix.

Similarly a matrix can act on a row vector from the left, by taking the dot product of the row vector with each column of the matrix,

$$ \mathbf{w}\mathrm{M} =
\begin{bmatrix}
x & y & z
\end{bmatrix}
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}
= \begin{bmatrix}
ax + dy + gz & bx + ey + hz & cx + fy + iz
\end{bmatrix}
$$

Once again the matrix-vector product is only well defined if the number of columns of the vector equals the number of rows of the matrix. The resulting vector has the same number of columns as the matrix.
The matrix-vector product is a linear transformation, i.e. it satisfies the properties of linearity, i.e. $\mathrm{M}(\mathbf{v} + \mathbf{w}) = \mathrm{M}\mathbf{v} + \mathrm{M}\mathbf{w}$ and $\mathrm{M}(k\mathbf{v}) = k\mathrm{M}\mathbf{v}$ for any scalar $k$.

Taking these two definitions together, we can see there is a natural way to multiply two matrices together, by taking the dot product of each row of the first matrix with each column of the second matrix,

$$ \mathrm{M}\mathrm{N} =
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}
\begin{bmatrix}
p & q & r \\
s & t & u \\
v & w & x
\end{bmatrix}
= \begin{bmatrix}
ap + bs + cv & aq + bt + cw & ar + bu + cx \\
dp + es + fv & dq + et + fw & dr + eu + fx \\
gp + hs + iv & gq + ht + iw & gr + hu + ix
\end{bmatrix}
$$

i.e.

$$ [\mathrm{M}\mathrm{N}]_{ij} = \sum_{k=1}^n M_{ik}N_{kj} $$

where $[\mathrm{M}\mathrm{N}]_{ij}$ is the $i,j$-th element of the resulting matrix, and $M_{ik}$ and $N_{kj}$ are the $i,k$-th and $k,j$-th elements of the matrices $\mathrm{M}$ and $\mathrm{N}$ respectively. The matrix-matrix product is only well defined if the number of columns of the first matrix equals the number of rows of the second matrix.

Importantly, matrix multiplication is not commutative, i.e. $\mathrm{M}\mathrm{N} \neq \mathrm{N}\mathrm{M}$ in general.

## Other basic operations on matrices

- **Addition**: Two matrices can be added together if they have the same dimensions, by adding the corresponding elements together,

  $$
  \mathrm{M} + \mathrm{N} =
  \begin{bmatrix}
  a & b & c \\
  d & e & f \\
  g & h & i
  \end{bmatrix}
  +
  \begin{bmatrix}
  p & q & r \\
  s & t & u \\
  v & w & x
  \end{bmatrix}
  =
  \begin{bmatrix}
  a + p & b + q & c + r \\
  d + s & e + t & f + u \\
  g + v & h + w & i + x
  \end{bmatrix}
  $$

- **Scalar multiplication**: A matrix can be multiplied by a scalar by multiplying each element of the matrix by the scalar,

  $$ k\mathrm{M} =
k\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix} 
=
\begin{bmatrix}
ka & kb & kc \\
kd & ke & kf \\
kg & kh & ki
\end{bmatrix}
  $$

  Unlike matrix multiplication, scalar multiplication is commutative, i.e. $k\kappa\mathrm{M} = \kappa k\mathrm{M}$.

- **Transpose**: The transpose of a matrix is obtained by flipping the matrix along its diagonal, i.e. reflecting the matrix in the line $y = x$. The transpose of a matrix is denoted by $\mathrm{M}^T$,

$$ \mathrm{M}^T =
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}^T
=
\begin{bmatrix}
a & d & g \\
b & e & h \\
c & f & i
\end{bmatrix}
$$

The transpose of a matrix has the property that $(\mathrm{M}^T)^T = \mathrm{M}$, and $(k\mathrm{M})^T = k\mathrm{M}^T$. Note that $(\mathbf{v}^T\mathbf{w}) = \mathbf{v}\cdot\mathbf{w}$, i.e. the dot product of two vectors is the same as the matrix product of the transpose of one vector with the other vector.

Other transposition identities include $(\mathrm{M} + \mathrm{N})^T = \mathrm{M}^T + \mathrm{N}^T$, and $(\mathrm{M}\mathrm{N})^T = \mathrm{N}^T\mathrm{M}^T$ Finally transposition is an involution, i.e. $(\mathrm{M}^T)^T = \mathrm{M}$.

## Non-square matrices

Matrices do not have to be square, i.e. the number of rows does not have to equal the number of columns. In this case, the matrix is said to have dimensions $m \times n$, where $m$ is the number of rows and $n$ is the number of columns. The matrix-vector product is still defined in the same way, and the matrix-matrix product is defined if the number of columns of the first matrix equals the number of rows of the second matrix.

## Matrices as linear transformations

### The identity matrix

The identity matrix is a square matrix with ones on the diagonal and zeros elsewhere. The identity matrix is denoted by $\mathrm{I}$, and has the property that $\mathrm{I}\mathbf{v} = \mathbf{v}$ for any vector $\mathbf{v}$. The identity matrix also acts as the multiplicative identity for matrices, i.e. $\mathrm{I}\mathrm{M} = \mathrm{M}\mathrm{I} = \mathrm{M}$ for any matrix $\mathrm{M}$.

### The zero matrix

The zero matrix is a matrix with all elements equal to zero. The zero matrix is denoted by $\mathrm{0}$, and has the property that $\mathrm{0}\mathbf{v} = \mathbf{0}$ for any vector $\mathbf{v}$. The zero matrix also acts as the additive identity for matrices, i.e. $\mathrm{M} + \mathrm{0} = \mathrm{M}$ for any matrix $\mathrm{M}$.

### Rotation and reflection matrices

The rotation matrix is a square matrix that rotates a vector by a given angle. The rotation matrix in 2D is given by

$$\mathrm{R}(\theta) =
\begin{bmatrix}
\cos(\theta) & -\sin(\theta) \\
\sin(\theta) & \cos(\theta)
\end{bmatrix}$$

where $\theta$ is the angle of rotation. Calculating the matrix-vector product of the rotation matrix with a vector $\mathbf{v}$ gives the rotated vector $\mathbf{v}'$,

$$
\mathbf{v}' = \mathrm{R}(\theta)\mathbf{v} =
\begin{bmatrix}
\cos(\theta) & -\sin(\theta) \\
\sin(\theta) & \cos(\theta)
\end{bmatrix}
\begin{bmatrix}
x \\
y
\end{bmatrix}
=
\begin{bmatrix}
x\cos(\theta) - y\sin(\theta) \\
y\sin(\theta) + x\cos(\theta)
\end{bmatrix}
$$

The reflection matrix is a square matrix that reflects a vector across a given line. The reflection matrix in 2D is given by
$$
\mathrm{R}(\theta) =
\begin{bmatrix}
\cos(2\theta) & \sin(2\theta) \\
\sin(2\theta) & -\cos(2\theta)
\end{bmatrix}
$$

where $\theta$ is the angle of the line of reflection. We can demonstrate this by applying a rotation matrix to map the line of reflection to the x-axis, then applying a reflection matrix across the x-axis, and finally applying the inverse rotation matrix to map back to the original coordinate system. The result is a reflection across the line at angle $\theta$. The x-axis reflection matrix is given by

$$
\mathrm{R}_x =
\begin{bmatrix}
1 & 0 \\
0 & -1
\end{bmatrix}
$$

and our rotation matrices are given by $R_1$ = R(-\theta) and $R_2$ = R(\theta). The product of these three matrices is given by
$$
R_1\mathrm{R}_x R_2 =
\begin{bmatrix}
\cos(-\theta) & \sin(\theta) \\
-\sin(\theta) & -\cos(\theta)
\end{bmatrix}
\begin{bmatrix}
1 & 0 \\
0 & -1
\end{bmatrix}
\begin{bmatrix}
\cos(\theta) & -\sin(\theta) \\
\sin(\theta) & \cos(\theta)
\end{bmatrix}
=  \begin{bmatrix}
\cos(\theta)^2 - \sin(\theta)^2  & 2\sin(\theta)\cos(\theta) \\
2\sin(\theta)\cos\theta & \sin(\theta)^2-\cos(\theta)^2
\end{bmatrix}
$$

### Scaling matrices

The scaling matrix is a square matrix that scales a vector by a given factor. The scaling matrix in 2D is given by

$$
\mathrm{S}(k) =
\begin{bmatrix}
k & 0 \\
0 & k
\end{bmatrix}
$$

where $k$ is the scaling factor. The scaling matrix scales a vector by multiplying each component of the vector by the scaling factor. For example, if we have a vector $\mathbf{v} = \begin{bmatrix} x \\ y \end{bmatrix}$, then the scaled vector $\mathbf{v}'$ is given by

## Matrix representation of systems of equations

Matrices can also be used to represent systems of linear equations. For example, the system of equations

$$
\begin{align*}
2x + 3y &= 5 \\
4x - y &= 3
\end{align*}
$$

can be written in matrix form as $\mathrm{A}\mathbf{x} = \mathbf{b}$, where

$$
\begin{aligned}
\mathrm{A} &= \begin{bmatrix}
2 & 3 \\
4 & -1
\end{bmatrix},\\
\mathbf{x} &= \begin{bmatrix}
x \\
y
\end{bmatrix},\\
\mathbf{b} &= \begin{bmatrix}
5 \\
3
\end{bmatrix}.
\end{aligned}
$$

We'll discuss this idea more in the next section.


## Further reading

For a more in-depth understanding of matrices, you can refer to the following resources:

- [Khan Academy: Matrices](https://www.khanacademy.org/math/precalculus/x9e81a4f98389efdf:matrices/x9e81a4f98389efdf:mat-intro/v/introduction-to-the-matrix)
- [Wikipedia: Matrix (mathematics)](https://en.wikipedia.org/wiki/Matrix_(mathematics))