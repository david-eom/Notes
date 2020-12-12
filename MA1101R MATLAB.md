#### Precision

`format long`: 16 dp

`format rat`: rational number

`format short`: 4 dp (default)

#### Basic Matrices

`size(A)`: $A_{m \times n}$, returns `ans  =  m  n`

`A(i, j)`: $A_{ij}$

`zeros(m, n)`: $0_{m \times n}$

`eye(n)`: $I_n$

`diag([a1, a2, ..., an])`: matrix with diagonal entries $a_1$ to $a_n$

#### ERO

`A(i, :) = c * A(i, :)`: $cA_{i}$

`A([i, j], :) = A([j, i], :)`: $A_{i} \leftrightarrow A_{j}$

`A(i, :) = A(i, :) + c * A(j, :)`: $A_{i}+cA_{j}$

#### Matrix Operations

`A'`: $A^T$

`rref(A)`: RREF

`A^n`: $A^n$

`A^-1 / inv(A)`: $A^{-1}$

#### Solve LS

`rref([A b])`: $Ax = b, \space (A|b)$

`syms s t u`: declare parameters

#### Span

`rref([S T])`: check whether $T \subseteq S$

#### Coordinate Vectors

`rref([S v])`: last column $(v)_S$

#### Rank and Nullspaces

`rank(A)`: $\text{rank}(A)$

`null(A, 'r')`: $\text{null}(A)$

#### Orthogonality

`dot(u, v)`: dot product

`norm(u)`: norm

`u = sym([...]); norm(u)`: exact value

`orth(E)`: orthonormal basis of column space of $E$

`E = sym([...]); orth(E)`: exact value

`orth(E, 'skipnormalization')`: orthogonal basis

#### Eigenvalues and Eigenvectors

`charpoly(A)`: characteristic polynomial, descending power

`charpoly(A, lambda) / det(lambda * eye(n) - A)`: charpoly in variable

`solve(ans) / roots(ans)`: solve charpoly in variable

`eig(A)`: eigenvalues

`null(lambda * eye(n) - A, 'r')`: eigenvectors (basis)

#### Diagonalisation

`A = sym(A); [P D] = eigh(A)`: obtain $P$ and $D$

`syms n; x(n) = P * D^n * P^-1 * x0`: find explicit formula for recurrence

`limit(x(n), n, inf)`: long term effect