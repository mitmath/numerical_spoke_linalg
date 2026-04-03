# Interdisciplinary Numerical Methods: Linear-Algebra "Spoke" 18.C21B/16.C21B

This repository holds materials the second 6-unit "spoke" half of a new MIT course (**Spring 2026**) introducing numerical methods and numerical analysis to a broad audience.   18.C21B/16.C21B covers **large-scale linear algebra**: what do you do when the matrices get so huge that you probably can't even store them as $n \times n$ arrays, much less run $O(n^3)$ algorithms like ordinary Gaussian elimination?

* **Prerequisites:** 18.03, 18.06, or equivalents, and some programming experience.   You should have taken the [first half-semester numerical "hub" 18.C21/16.C21](https://github.com/mitmath/numerical_hub), or alternatively *any other introductory numerical-methods course* (e.g. 18.330, 18.C25, 18.C20, 16.90, 2.086, 12.010, 6.7330J/16.920J/2.097J, or 6.S955).

Taking both the hub and any spoke will count as an 18.3xx class for math majors, similar to 18.330, and as 16.90 for course-16 majors.

**Instructor**: [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj).

**Lectures**: MWF10 in 2-142 (Mar 30 – May 11, slides and notes posted below.

**Grading** (all assignments **submitted electronically** via [Gradescope on Canvas](https://canvas.mit.edu/courses/36179)):
* 30% **4 weekly psets:** due Wednesdays at midnight: April 8, 15, 22, and 29, 10% **pset check-ins** (oral check-ins on selected psets where they have to explain their work; pass/fail per problem).
* 30% **final project**: due May 11 (last day of class). The final project will be an 8–15 page paper reviewing, implementing, and testing some interesting numerical linear-algebra algorithm not covered in the course.  A 1-page *proposal* will be due April 17 (but you are encouraged to submit earlier).  See [final-project/proposal information](psets/proposal.md).  20% **final project presentation** in-class (last week).
* 10% **attendance**

**Collaboration policy:** Talk to anyone you want to and read anything you want to, with two caveats: First, make a solid effort to solve a problem on your own before discussing it with classmates or googling. Second, no matter whom you talk to or what you read, write up the solution on your own, without having their answer in front of you (this includes ChatGPT and similar). (You can use [psetpartners.mit.edu](https://psetpartners.mit.edu/) to find problem-set partners.)

**Teaching Assistant**: [Rodrigo Arietta Candia](https://math.mit.edu/directory/profile.html?pid=2409)

**Office Hours**: Wednesday 3pm in 2-345 (Prof. Johnson)

**Resources**: [Piazza discussion forum](https://piazza.com/class/mncb6dzy8jj4bc/), [math learning center](https://math.mit.edu/learningcenter/), [pset partners](https://psetpartners.mit.edu/).

**Textbook**: No required textbook, but suggestions for further reading will be posted after each lecture.  The book [*Fundamentals of Numerical Computation* (FNC)](https://fncbook.com/) by Driscoll and Braun is **freely available online**, has examples in Julia, Python, and Matlab, and is a valuable resource.   Another important textbook for the course is [_Numerical Linear Algebra_ by Trefethen and Bau](http://www.amazon.com/Numerical-Linear-Algebra-Lloyd-Trefethen/dp/0898713617). ([Readable online](http://owens.mit.edu/sfx_local?bookid=9436&rft.genre=book&sid=Barton:Books24x7) with MIT certificates, and there is [also a PDF posted online at uchicago](https://www.stat.uchicago.edu/~lekheng/courses/309/books/Trefethen-Bau.pdf), though this is a graduate-level textbook and hence is somewhat more sophisticated than the coverage in this course.)

This document is a *brief* summary of what was covered in each
lecture, along with links and suggestions for further reading.  It is
*not* a good substitute for attending lecture, but may provide a
useful study guide.

## Lecture 1 (March 31)

* Overview and syllabus (this web page).
* [Julia notebook with scaling examples](notes/linalg-scaling.ipynb)

Reviewed the fact that traditional "dense" linear-algebra algorthms (factorizations LU, QR, diagonalization, SVD, etc.), which assume little or no special structure of the matrix, typically require $O(m^3)$ cost and $O(m^2)$ memory.   In practice, this means that $m=1000$ matrices are easy laptop-scale problems, but much bigger matrices like $m=10^6$ seem out of reach.

However, huge matrices ($m=10^6$ or more) commonly arise in many applications, such ans engineering models of partial differential equations (where functions are discretized onto a grid or mesh).   A $100 \times 100 \times 100$ mesh is a *small* model of a 3d system, and that has $10^6$ unknowns.   How do people handle such problems?

The trick is that huge matrices typically have some special structure that you can exploit, and the most common such structure is **[sparsity](https://en.wikipedia.org/wiki/Sparse_matrix): the matrix entries are mostly zero**.  Ideally, an $m \times m$ sparse matrix should only have $O(m)$ nonzero entries.  More generally, an $m \times n$ array $A$ of numbers, a "matrix", is just one way to represent a [linear operator](https://en.wikipedia.org/wiki/Linear_map) — more generally, there are many other ways to represent the operation $x \mapsto Ax$ (or $y \mapsto A^T y$), and many types of linear operations can be represented with less storage and time than $mn$ numbers.  There are not only sparse matrices, but also things like convolutions, low-rank matrices, linear combinations thereof, and more.

Showed how a sparse matrix, in fact a symmetric [tridiagonal matrix](https://en.wikipedia.org/wiki/Tridiagonal_matrix) arises from discretizing a simple PDE on a grid with finite differences: [Poisson's equation](https://en.wikipedia.org/wiki/Poisson%27s_equation) $d^2 u / dx^2 = f$.   (Sparsity arises because differential operators like $\nabla^2$ are *local* operators in space, so on a grid or mesh they only couple neighboring elements.)   Computationally, we can store and solve such a matrix *very* efficiently, with $O(m)$ work.

How can we generalize this to other sparsity patterns and other types of large-scale matrix structures?

**Further reading**: FNC book [section 8.1: sparsity and structure](https://fncbook.com/structure-1).  The example of discretizing a 1d Poisson equation with finite differences, resulting in a tridiagonal matrix, is discussed in many sources, e.g. [these UTexas course notes](https://farside.ph.utexas.edu/teaching/329/lectures/node62.html).

## Lecture 2 (April 1)

* Sparse matrices and data structures
* [sparse-matrix slides from 18.335 (Fall 2006)](https://github.com/mitmath/18335/blob/spring21/notes/lec21handout6pp.pdf)
* [Julia notebook on dense and sparse matrices](https://nbviewer.org/github/mitmath/1806/blob/fall22/notes/Dense-and-Sparse.ipynb) from 18.06 (Fall 2022)
* [pset 1](psets/pset1.ipynb): due Wed, Apr 8 at midnight

**Further reading:** Sparse matrices in CSC format, along with sparse-direct algorithms, are provided in Julia by the [SparseArrays standard library](https://docs.julialang.org/en/v1/stdlib/SparseArrays/), and in Python by [`scipy.sparse`](https://docs.scipy.org/doc/scipy/reference/sparse.html), along with additional packages that provide other algorithms and data structures; for example, a famous C library for this is [PETSc](https://petsc.org/release/), which also has Python and Julia interfaces, and there are many sparse-direct libraries such as [MUMPS](https://mumps-solver.org/index.php) and [Pardiso](https://www.intel.com/content/www/us/en/docs/onemkl/developer-reference-c/2023-0/pardiso.html). Sparse-direct solvers are described in detail by the book *Direct Methods for Sparse Linear Systems* by Davis; the corresponding software library is Davis's [SuiteSparse](https://github.com/DrTimothyAldenDavis/SuiteSparse), which is used by SparseArrays in Julia and is available in Python via [`scikit-sparse`](https://github.com/scikit-sparse/scikit-sparse).  We will soon start talking about *iterative* methods; more advanced treatments include the book *Numerical Linear Algebra* by Trefethen and Bao, and surveys of algorithms can be found in the *Templates* books for [Ax=b](http://www.netlib.org/linalg/html_templates/Templates.html) and [Ax=λx](http://web.cs.ucdavis.edu/~bai/ET/contents.html).  [Some crude rules of thumb](https://github.com/mitmath/18335/blob/spring20/notes/solver-options.pdf) for solving linear systems (from 18.335 spring 2020) may be useful.

## Lecture 3 (April 3)

Iterative methods: the big picture is to solve $Ax=b$, $Ax = \lambda x$ and similar methods by iteratively improving a "guess" (usually starting with $x=0$ or random numbers), where the matrix $A$ is *only* used via matrix–vector products $Av$.

* **Pro:** can be fast whenever $Av$ is fast (e.g. if $A$ is sparse, low-rank, [Toeplitz](https://en.wikipedia.org/wiki/Toeplitz_matrix), etc.).  Can scale to huge probems.
* **Con:** hard to design an iteration that converges quickly, how well the methods work is often problem-dependent, often requires problem-depending tuning and experimentation (e.g. [preconditioners](https://en.wikipedia.org/wiki/Preconditioner))

The simplest iterative method is the [power method for eigenvalues](https://en.wikipedia.org/wiki/Power_iteration): repeatedly multipling a random initial vector by $A$ converges to an eigenvector of the largest $|\lambda|$, *if* there is no other eigenvalue with equal magnitude.  Showed an example (easiest for real-symmetric $A=A^T$ matrices).

Analyzed the convergence of the power method: if $\lambda_1$ is the biggest-magnitude eigenvalue and $\lambda_2$ is the second-biggest, then the error in the eigenvector on the k-th iteration is $O(|\lambda_2/\lambda_1|^k)$.

Given an estimate $x$ for an eigenvector, a nice way to get an estimate for the corresponding eigenvalue is the [Rayleigh quotient](https://en.wikipedia.org/wiki/Rayleigh_quotient) $\lambda \approx x^T A x / x^T x$.   Numerically, showed that the error in this eigenvalue estimate is the *square* of the error in $x$ for real-symmetric $A$, and in fact the eigenvalue error converges as $O(|\lambda_2/\lambda_1|^{2k})$ in this case.  (We will see *why* this happens next time.)

To find *other* eigenvectors and eigenvalues, one possibility is an algorithm called **deflation**. It exploits the fact that for real-symmetric $A$, the eigenvectors $q_1, q_2, \ldots$ for distinct $\lambda$ are orthogonal.   So, once we have found $q_1$, we can repeat the power method but **project each step to be orthogonal** to the previously found eigenvector, i.e. replace $x \longleftarrow x - q_1 (q_1^T x)$.  This will then converge to $q_2$ (for the second-biggest $|\lambda|$).  To get $q_3$, repeat the power method but project orthogonal to both $q_1, q_2$ with $x \longleftarrow x - q_1 (q_1^T x) - q_2 (q_2^T x)$, etcetera.

Deflation is a terrible scheme if you want the *smallest* magnitude eigenvalue, however, since you'd have to compute *all* the other eigenvalues/vectors first.   Instead, to find the smallest |λ| one can simply apply the power method to $A^{-1}$: on each step, compute $y = A^{-1} x$, i.e. solve $Ay = x$.  This is called [(unshifted) inverse iteration](https://en.wikipedia.org/wiki/Inverse_iteration).   It relies on a fast way to solve $Ay = x$; for example, if $A$ is sparse, you can compute the sparse LU factorization and re-use it on each step with a sparse-direct solve.   In general, we will see that solution methods for eigenproblems and linear systems are often closely related!

**Further reading:** FNC book [section 8.2: power iteration](https://fncbook.com/power) and [section 8.3: inverse iteration](https://fncbook.com/inviter). Trefethen & Bau, lecture 27.  See [this notebook on the power method](https://github.com/mitmath/1806/blob/fall22/notes/Power-Method.ipynb) from 18.06.
