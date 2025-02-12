# Vector Spaces

1. Commutativity: $\vec{v} + \vec{w} = \vec{w} + \vec{v} \quad \forall \vec{v}, \vec{w} \in V$
1. Associativity: $(\vec{u} + \vec{v}) + \vec{w} = \vec{u} + (\vec{v} + \vec{w}) \quad \forall \vec{u}, \vec{v}, \vec{w} \in V$
1. Zero Vector:   $\vec{v} + \vec{0} = \vec{v} \quad \forall \vec{v} \in V$
1. Additive Inverse: $\vec{v} + \vec{w} = \vec{0} \quad \vec{w}$ is  denoted $-\vec{v}$
1. Multiplicative Identity: $1 \vec{v} = \vec{v} \quad \forall \vec{v}$
1. Multiplicative Associativity: $1 \vec{v} = \vec{v} \quad \forall \vec{v} \in V$
1. Distributivity Over Vector Sums: $\alpha {\vec{u} + \vec{v}} = \alpha \vec{u} + \alpha \vec{v} \quad \forall \vec{u}, \vec{v} \in V$ and all scalars $\alpha$
1. Distributivity Over Scalar Sums: $(\alpha + \beta) \vec{v} = \alpha \vec{u} + \beta \vec{v} \quad \forall \vec{v}$ and all scalars $\alpha, \beta$

# System of Vectors

A set of vectors belonging to the same vector space.

# Basis

A system of vectors where any $\vec{v} \in V$ has a _unique_ representation as
a linear combination of the vectors in the basis:
$$\vec{v} = \sum_{k=1}^n \alpha_k \vec{v}_k$$

A basis is both a **generating system** and **linearly independent**. The
coefficients are called the _coordinates_ of the vector $\vec{v}$ in the basis.

## Standard Basis

The system $\vec{e}_1, \vec{e}_2, ..., \vec{e}_n \in \mathbb{F}^n$
$$
\vec{e}_1 = \begin{pmatrix}
1 \\
0 \\
0 \\
\vdots \\
0 \\
\end{pmatrix},
\vec{e}_2 = \begin{pmatrix}
0 \\
1 \\
0 \\
\vdots \\
0 \\
\end{pmatrix},
...,
\vec{e}_n = \begin{pmatrix}
1 \\
0 \\
0 \\
\vdots \\
1 \\
\end{pmatrix}
$$

# Generating System

A system of vectors where any $\vec{v} \in V$ has _a_ representation (not
necessarily unique) as a linear combination of the vectors in the system.

# Linear Independence

A system of vectors is linearly independent iff the trivial linear combination
is the only one that equals $\vec{0}$:
$$
\vec{0} = \sum_{k=1}^n \alpha_k \vec{v}_k \quad \quad \text{where} \sum_{k=1}^n |\alpha_k| = 0
$$

# Trivial Linear Combination

A linear combination where the coefficients are all $0$ ($\vec{v}$ will always
be $\vec{0}$):

$$
\vec{v} = \sum_{k=1}^n 0 \vec{v}_k
$$

# Linear Dependence

A system of vectors is linearly dependent if $\vec{0}$ can be represented as a
_non-trivial_ linear combination (at least one coefficient is non-zero):
$$
\vec{0} = \sum_{k=1}^n \alpha_k \vec{v}_k \quad \quad \text{where} \sum_{k=1}^n |\alpha_k| \ne 0
$$

A system of vectors is linearly dependent if one of the vectors can be
represented as a linear combination of the others.

# Transformation

A transformation from set $X$ to set $Y$ assigns to each $x \in X$, a value
$y \in Y$.

The set $X$ is called the _domain_. The set $Y$ is called the _codomain_ or
_target space_. "Transformation", "map", and "operator" all mean the same
thing.

# Linear Transformation

A linear transformation $T$ is one where:
$$
T(\vec{u} + \vec{v}) = T(\vec{u}) + T(\vec{v}) \quad \forall \vec{u},\vec{v} \in V \\
T(\alpha \vec{v}) = \alpha T(\vec{v}) \quad \forall \vec{v} \in V ~ \text{and all scalars} ~ \alpha
$$
Or:
$$
T(\alpha \vec{u} + \beta \vec{v}) = \alpha T(\vec{u}) + \beta T(\vec{v}) \quad \forall \vec{u},\vec{v} \in V  ~ \text{and all scalars} ~ \alpha, \beta \\
$$

Completely defined by its effect on a generating system or basis.

