#math #linear_algebra 

# Determinants and Inverses

## Determinants

The determinant of a square matrix is a scalar function of its elements, and is denoted by $\det(\mathrm{M})$ or $|\mathrm{M}|$. The determinant of a $2 \times 2$ matrix, $\mathbf{M} = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$, is given by $ad - bc$. We can define the determinant of a $3 \times 3$ matrix by expanding along the first row recursively,

$$ \det(\mathrm{M}) := \left|
\begin{array}{ccc}
a & b & c \\
d & e & f \\
g & h & i
\end{array}
\right| = a\left|
\begin{array}{cc}
e & f \\
h & i
\end{array}
\right| - b\left|
\begin{array}{cc}
d & f \\
g & i
\end{array}
\right| + c\left|
\begin{array}{cc}
d & e \\
g & h
\end{array}
\right| = aei - afh - bdi + bfg + cdh - ceg
$$

Where the derminants of the $2 \times 2$ matrices are given by the formula above. Note that under appropriate permutations of signs, a similar formula exist expanding along any row or column, and will hold for $4 \times 4$ matrices and higher. This can be written as a sum over all permutations of the indices, $\sum_{\sigma} \mathrm{sgn}(\sigma) \prod_{i=1}^{n} \mathrm{M}_{i\sigma(i)}$, where $\sigma$ is a permutation of the indices, $\mathrm{sgn}(\sigma)$ is the sign of the permutation, and $\mathrm{M}_{ij}$ is the matrix obtained by deleting the $i$th row and $j$th column of $\mathrm{M}$.

The determinant satisfies the following properties:

- $\det(\mathrm{M}^T) = \det(\mathrm{M})$
- $\det(k\mathrm{M}) = k^n\det(\mathrm{M})$
- $\det(\mathrm{M}\mathrm{N}) = \det(\mathrm{M})\det(\mathrm{N})$
- $\det(\mathrm{M}^{-1}) = \frac{1}{\det(\mathrm{M})}$

The determinant of a matrix can be used to determine if a matrix is invertible, i.e. for a square matrix if $\det(\mathrm{M}) \neq 0$, then $\mathrm{M}^{-1}$ exists.


## Inverses

The inverse of a square matrix $\mathrm{M}$ is denoted by $\mathrm{M}^{-1}$, and is defined such that $\mathrm{M}\mathrm{M}^{-1} = \mathrm{I}$, where $\mathrm{I}$ is the identity matrix, i.e. a matrix with ones on the diagonal and zeros elsewhere. By associativity of matrix multiplication, we can see that $\mathrm{M}^{-1}\mathrm{M} = \mathrm{I}$ as well.

If it exists, the inverse of a $2 \times 2$ matrix $\mathrm{M} = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$ is given by

$$ \mathrm{M}^{-1} = \frac{1}{ad - bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix} $$

where $ad - bc$ is the determinant of the matrix. For a $3 \times 3$ matrix, the inverse is given by

$$ \mathrm{M}^{-1} = \frac{1}{\det(\mathrm{M})}\begin{bmatrix}
ei - fh & ch - bi & bf - ce \\
fg - di & ai - cg & cd - af \\
dh - eg & bg - ah & ae - bd
\end{bmatrix} $$

where the matrix on the right-hand side is the _adjugate_ of the matrix, i.e. the transpose of the matrix of _cofactors_ (i.e. the determinant of the matrix obtained by deleting the $i$th row and $j$th column of $\mathrm{M}$, multiplied by $(-1)^{i+j}$). This formula can be generalised to higher dimensions, but number of terms grows rapidly with the size of the matrix, so it is not practical to write down the invers of a large matrix by hand.

The inverse of a matrix can be used to solve systems of linear equations, taking advantage of the matrix representation of the system. If we have a system of equations $\mathrm{A}\mathbf{x} = \mathbf{b}$, then we can multiply both sides by $\mathrm{A}^{-1}$ to get $\mathbf{x} = \mathrm{A}^{-1}\mathbf{b}$. Where an inverse is known this can be a quick way to solve a system of equations, but in general it is computationally expensive to calculate the inverse of a matrix, and there are more efficient methods to solve systems of equations.

Turning this problem the other way around, if we have a matrix $\mathrm{A}$, we can find its inverse by solving the system of equations $\mathrm{A}\mathrm{A}^{-1} = \mathrm{I}$, where $\mathrm{I}$ is the identity matrix. This can be done by solving the system of equations for each column of the identity matrix in turn, i.e. $\mathrm{A}\mathbf{x}_i = \mathbf{e}_i$, where $\mathbf{e}_i$ is the $i$th column of the identity matrix. This means any direct method for solving systems of equations can also be applied to find the inverse of a matrix.