#math #linear_algebra 

# Vector Spaces

This section is more mathematical than the previous sections, and is intended to give a brief overview of the concept of vector spaces. It is not intended to be a comprehensive treatment of the subject, but rather a starting point for further study. Don't worry if you don't understand everything at first; the important thing is to get a feel for the concepts and ideas involved. If you are interested in learning more about vector spaces, there are many excellent textbooks and online resources available.

## Introduction

To a mathematician, a **vector space** is a set of elements, $\mathcal{V}$, along with a scalar field, $F$, (e.g the real numbers or complex numbers) and two operations, vector addition and scalar multiplication, that together satisfy a set of axioms. The elements of the set are called **vectors**. The axioms are as follows:

### Axioms of a vector space

1. **Closure under addition**: If $\mathbf{v}$ and $\mathbf{w}$ are vectors in the vector space, then $\mathbf{v} + \mathbf{w}$ is also in the vector space. i.e.

$$ \mathbf{v}, \mathbf{w} \in \mathcal{V} \implies \mathbf{v} + \mathbf{w} \in \mathcal{V}.$$
2. **Associativity of addition**: For all choices of 3 vectors $\mathbf{u}$, $\mathbf{v}$, and $\mathbf{w}$ from the vector space, $(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w})$.
3. **Commutativity of addition**: For all vectors $\mathbf{v}$ and $\mathbf{w}$ in the vector space, $\mathbf{v} + \mathbf{w} = \mathbf{w} + \mathbf{v}$.
4. **Additive identity**: There exists a vector $\mathbf{0}$ in the vector space such that for all vectors $\mathbf{v}$ in the vector space, $\mathbf{v} + \mathbf{0} = \mathbf{v}$.
5. **Additive inverse**: For every vector $\mathbf{v}$ in the vector space, there exists a vector $-\mathbf{v}$ in the vector space such that $\mathbf{v} + (-\mathbf{v}) = \mathbf{0}$.
6. **Closure under scalar multiplication**: If $\mathbf{v}$ is a vector in the vector space and $k\in F$ is a scalar, then $k\mathbf{v}$ is also in the vector space.
7. **Distributivity of scalar multiplication with respect to vector addition**: For all vectors $\mathbf{v}$ and $\mathbf{w}$ in the vector space and all scalars $k\in F$, $k(\mathbf{v} + \mathbf{w}) = k\mathbf{v} + k\mathbf{w}$.
8. **Distributivity of scalar multiplication with respect to scalar addition**: For all vectors $\mathbf{v}$ in the vector space and all scalars $k$ and $l$ in $F$, $(k + l)\mathbf{v} = k\mathbf{v} + l\mathbf{v}$.
9. **Compatibility of scalar multiplication with scalar multiplication**: For all vectors $\mathbf{v}$ in the vector space and all scalars $k$ and $l$, $(kl)\mathbf{v} = k(l\mathbf{v})$.
10. **Identity element of scalar multiplication**: For all vectors $\mathbf{v}$ in the vector space, $1\mathbf{v} = \mathbf{v}$.

This definition includes both the familiar geometric vectors in $\mathbb{R}^n$ for the scalar field $\mathbb{R}$, as well as many more abstract objects such as functions, polynomials, and matrices.

## Basis and dimension

A **basis** for a vector space is a set of vectors that are linearly independent and which span the vector space. A set of vectors, $\mathcal{V}$ is **linearly independent** if no vector in the set can be written as a linear combination of the other vectors in the set (i.e. ) for each $\mathbf{v}_i \in $\mathcal{V}$ the is no combination $$\sum_{j\neq i} a_j\mathbf{v}_j = $$ for $a_i\in F$, $\mathbf{v}_j \in \mathcal{V}$.

A set of vectors **spans** a vector space if every vector in the vector space can be written as a linear combination of the vectors in the set. The **dimension** of a vector space is then defined as the number of vectors in a basis for the vector space (it can be proven all possible choices of basis have the same number of members). For finite-dimensional vector spaces, this is also the number of entries in its column representation. For example, the dimension of $\mathbb{R}^n$ is $n$.

## Example: Vectors in $\mathbb{R}^n$

The set of vectors in $\mathbb{R}^n$ is a vector space. The scalar field is usually $\mathbb{R}$, the set of real numbers. The vectors can be represented as $n$-tuples of real numbers, i.e. $\mathbf{v} = (v_1, v_2, \ldots, v_n)$. The operations are:

- **Addition**: If $\mathbf{v} = (v_1, v_2, \ldots, v_n)$ and $\mathbf{w} = (w_1, w_2, \ldots, w_n)$, then $\mathbf{v} + \mathbf{w} = (v_1 + w_1, v_2 + w_2, \ldots, v_n + w_n)$.
- **Scalar multiplication**: If $k$ is a scalar and $\mathbf{v} = (v_1, v_2, \ldots, v_n)$, then $k\mathbf{v} = (kv_1, kv_2, \ldots, kv_n)$.

The axioms are all satisfied, mostly through well-know properties of real numbers, For example, the additive identity is the zero vector, $\mathbf{0} = (0, 0, \ldots, 0)$, and the additive inverse of $\mathbf{v} = (v_1, v_2, \ldots, v_n)$ is $-\mathbf{v} = (-v_1, -v_2, \ldots, -v_n)$.

This is a finite-dimensional vector space, and a natural basis is given by the standard basis vectors, $\mathbf{e}_1 = (1, 0, \ldots, 0)$, $\mathbf{e}_2 = (0, 1, \ldots, 0)$, $\ldots$, $\mathbf{e}_n = (0, 0, \ldots, 1)$.

## Example: Polynomials

The set of polynomials of degree at most $n$ is a vector space. The corresponding scalar field is usually  $\mathbb{R}$, the set of real numbers, although things also work with $mathbb{C}, the set of complex numbers. The vectors (i.e. the elements of the space) are polynomials of degree at most $n$. The operations are:

- **Addition**: If $p(x) = a_0 + a_1x + a_2x^2 + \ldots + a_nx^n$ and $q(x) = b_0 + b_1x + b_2x^2 + \ldots + b_nx^n$, then $p(x) + q(x) = (a_0 + b_0) + (a_1 + b_1)x + (a_2 + b_2)x^2 + \ldots + (a_n + b_n)x^n$.
- **Scalar multiplication**: If $k$ is a scalar and $p(x) = a_0 + a_1x + a_2x^2 + \ldots + a_nx^n$, then $kp(x) = ka_0 + ka_1x + ka_2x^2 + \ldots + ka_nx^n$.

The axioms are satisfied. For example, the additive identity is the zero polynomial, $0 = 0 + 0x + 0x^2 + \ldots + 0x^n$, and the additive inverse of $p(x) = a_0 + a_1x + a_2x^2 + \ldots + a_nx^n$ is $-p(x) = -a_0 - a_1x - a_2x^2 - \ldots - a_nx^n$.

This is another finite-dimensional vector space, and one common basis is given by the set of polynomials $1, x, x^2, \ldots, x^n$. under this choice of basis, the dimension of the space is $n + 1$. If $n$ is 2, we could write a polynomial (say $p(x) = 2 + 3x + 4x^2 $) in this basis as 

$$ p(x) = \begin{bmatrix}
2 \\
3 \\
4
\end{bmatrix}$$

A change of basis acts like a change of coordinate for geometric vectors. For example, if we change to the basis $[x^2+x, 1-x^2, x^2-x]$, then the polynomial $p(x) = 2 + 3x + 4x^2$ would be represented as
$$ p(x) = \begin{bmatrix}
\frac{9}{2}\\
2\\
\frac{3}{2}
\end{bmatrix}$$

As with the arrows, different choices of basis will give different column representations of the same polynomial. 
## Example: Continuous functions

The set of continuous functions on the interval $[a, b]$ forms a vector space. The scalar field once again usually taken as $\mathbb{R}$, the set of real numbers. The vectors are continuous functions on the interval $[a, b]$. The operations are normally defined as follows:

- **Addition**: If $f(x)$ and $g(x)$ are continuous functions on the interval $[a, b]$, then their addition is definted pointwise through $(f + g)(x) = f(x) + g(x)$. It is not too tricky to show that this is also continuous.

- **Scalar multiplication**: If $k$ is a scalar and $f(x)$ is a continuous function on the interval $[a, b]$, then their scalar multiplication is defined pointwise through $(kf)(x) = k(f(x))$. This is also continuous. The proof is again not too tricky.

With these definitions, the axioms are all satisfied. For example, the additive identity is the zero function, $z(x) = 0$ for all $x$, and the additive inverse of $f(x)$ is the function $-f(x)$.

Unlike our previous examples, this vector space is infinite-dimensional, i.e. there is no finite-sized basis generating the space of functions through linear combinations. This means there is no neat column representation of the functions in this space.

## Subspaces

A **subspace** of a vector space is a subset of the vector space that is itself a vector space. A subspace must contain the zero vector, and be closed under addition and scalar multiplication. For finite dimensional vector spaces, we can form subspaces by taking the span of incomplete sets of basis vectors. For example, the $x$-$y$ plane in $\mathbb{R}^3$ is a subspace of $\mathbb{R}^3$.

## Exercises

1. Consider the following definitions of "addition" and "scalar multiplication" for positive real numbers $a$ and $b$:
    - Addition: $a \oplus b = \exp(\ln a + \ln b)$
    - Scalar multiplication: $k \odot a = \exp(k \ln a) $ for $k \in \mathbb{R}$.
    
    Show that this defines a vector space (i.e. convince yourself that all the axioms apply). What is the dimension of this vector space?
    
    ```{dropdown} Answer

    All the vector space axioms are satisfied here:
    
    - Closure is satisfied because the exponential function is defined for all real numbers (returning a positive real number) and the logarithm is defined for all positive real numbers (returning a real number), so our set is closed under $\odot$ and $\oplus$.
    - Associativity of addition: $a \oplus (b \oplus c) = \exp(\ln a + \ln(\exp(\ln b + \ln c))) = \exp (\ln a + \ln b + \ln c) = \exp(\ln(\exp*(\ln a + \ln b) + \ln c)) = (a \oplus b) \oplus c$.
    - Commutativity of addition: $a \oplus b = \exp(\ln a + \ln b) = \exp(\ln b + \ln a) = b \oplus a$.
    - Additive identity: $a \oplus 1 = \exp(\ln a + \ln 1) = \exp(\ln a + 0) = a$.
    - Additive inverse: $a \oplus \exp(-\ln a) = \exp(\ln a + \ln(\exp(-\ln a))) = \exp(\ln a + 0) = a$.
    - Distributivity of scalar multiplication $k \odot (a \oplus b) = k \odot \exp(\ln a + \ln b) = \exp(k\ln(\exp(\ln a + \ln b))) = \exp(k\ln a + k\ln b) = \exp(\ln(\exp(k\ln a) + \exp(k\ln b))) = k \odot a \oplus k \odot b$.
    - $1 \odot a = \exp(1 \ln a) = \exp(\ln a) = a$.

    The dimension of this vector space is 1, since we can use the basis $\{2\}$ (or any other positive number not equal to 1) generate all positive real numbers through scalar multiplication, $a = (\frac{\ln a}{\ln 2}) \odot 2$.

    ```