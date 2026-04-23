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

## Lecture 4 (April 6)

* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0) from spring 2025, page 29+

If you want to find the smallest |λ| instead of the biggest, you can simply apply the power method to $A^{-1}$: on each step, compute $y = A^{-1} x$, i.e. solve $Ay = x$.  This is called [(unshifted) inverse iteration](https://en.wikipedia.org/wiki/Inverse_iteration).   It relies on a fast way to solve $Ay = x$; for example, if $A$ is sparse, you can compute the sparse LU factorization and re-use it on each step with a sparse-direct solve.   In general, we will see that solution methods for eigenproblems and linear systems are often closely related!

Proved that, for a real-symmetric (Hermitian) matrix A=Aᵀ, the [Rayleigh quotient R(x)=xᵀAx/xᵀx](https://en.wikipedia.org/wiki/Rayleigh_quotient) is bounded above and below by the largest and smallest eigenvalues of A (the ["min–max theorem"](https://en.wikipedia.org/wiki/Min-max_theorem)).   This theorem is useful for lots of things in linear algebra.  Here, it helps us understand why the Rayleigh quotient is so accurate: the power method converges to a maximum-|λ| eigenvalue, which is either the smallest (most negative) or the largest (most positive) λ of a real-symmetric A, and hence that λ is an *extremum* (minimum or maximum) of the Rayleigh quotient where its gradient is zero.  In fact, you can show that ∇R=0 for *any* eigenvector (not necessarily min/max λ).   This means, if we Taylor expand R(x+δx) around an eigenvector x where R(x)=λ, you get R(x+δx)=λ+O(‖δx‖^2): the error in the eigenvalue λ goes as the *square* of the error in the eigenvector (for real-symmetric A).


Above, we considered inverse iteration.  A more general idea is **shifted inverse iteration**: at each step, compute $(A - \mu I)^{-1}$ times the previous step, for some shift $\mu$.   This converges to an eigenvector of the λ **closest to μ**.   If μ is very close to an eigenvalue, it converges extremely quickly.   Not only does this allow you to search for eigenvalues anywhere in the spectrum that you want, but it also gives you a way to accelerate iteration if you have a good guess for λ.

But where would you get a good guess for λ?  A simple answer is to use the Rayleigh quotient R(x), where x comes from previous steps of the power iteration.  Even if the power iteration is converging slowly, once you have even a rough approximation for λ you can use it as a shift.  This leads to the algorithm of [Rayleigh-quotient iteration](https://en.wikipedia.org/wiki/Rayleigh_quotient_iteration): at each step, compute $x_k = (A - \mu_k I)^{-1} x_{k-1} / \Vert \cdots \Vert$, where $\mu_k = R(x_{k-1})$.   It turns out that this converges *faster* than exponentially with $k$: for a Hermitian problem, it *cubes the error* (triples the number of digits) at every step, once you get close enough to the eigenvalue.  (This is even faster than the quadratic convergence of Newton's method!)  For a non-Hermitian problem, it has quadratic convergence similar to Newton.

The big problem with Rayleigh-quotient iteration, like Newton's method, is the need for a good initial guess — if you have a bad initial guess, it can be quite unpredictable what eigenvalue it converges to!   But any time you can obtain a rough idea of where the desired eigenvalue is, it means you can zoom into the exact value extremely quickly.

**Further reading:** FNC book [section 8.3: inverse iteration](https://fncbook.com/inviter); however, beware that the book currently shows a less accurate (for real-symmetric/Hermitian A) method to estimate eigenvalues (issue [fnc#16](https://github.com/fncbook/fnc/issues/16)).  [Trefethen & Bau, lecture 27](https://www.cs.cmu.edu/afs/cs/academic/class/15859n-f16/Handouts/TrefethenBau/RayleighQuotient-27.pdf) covers these algorithms in much more depth.  [These slides by Per Persson (2006)](https://github.com/mitmath/18335/blob/spring21/notes/lec15handout6pp.pdf) are a useful summary.

## Lecture 5 (April 8)
* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0) from spring 2025, page 35+
* [pset 1 solutions](psets/pset1sol.ipynb)
* [pset 2](psets/pset2.ipynb): due April 15

To find *other* eigenvectors and eigenvalues of a Hermitian problem, one possibility is an algorithm called **deflation**. It exploits the fact that for real-symmetric $A$, the eigenvectors $q_1, q_2, \ldots$ for distinct $\lambda$ are orthogonal.   So, once we have found $q_1$, we can repeat the power method but **project each step to be orthogonal** to the previously found eigenvector, i.e. replace $x \longleftarrow x - q_1 (q_1^T x)$.  This will then converge to $q_2$ (for the second-biggest $|\lambda|$).  To get $q_3$, repeat the power method but project orthogonal to both $q_1, q_2$ with $x \longleftarrow x - q_1 (q_1^T x) - q_2 (q_2^T x)$, etcetera.

Introduced Krylov subspaces, and the idea of **Krylov subspace** methods: ideally, we want to find the "best" solution in the *whole subspace* 𝒦ₙ spanned by {x₀,Ax₀,...,Aⁿ⁻¹x₀}, which is the *only* subspace you can get starting from x₀ if you are only allowed linear operations and matrix–vector products.

The **Arnoldi** algorithm is a Krylov algorithm for eigenproblems.  It basically has two components:

1. Find an orthonormal basis Qₙ for 𝒦ₙ.   Essentially, we will to this by a form of Gram–Schmidt, to be determined.
2. Given the basis, give the "best" estimate in 𝒦ₙ for one or more eigenvectors and eigenvalues.

How do we construct the orthonormal basis $Q_n$ of the Krylov space?  Reviewed the [Gram–Schmidt](https://en.wikipedia.org/wiki/Gram%E2%80%93Schmidt_process) algorithm, along with its numerically stable cousin, modified Gram–Schmidt.  Described how a variation on this idea can be used for the Krylov space: at each step, take the most recent orthonormal basis vector $q_n$, multiply it by $A$ to get $v = Aq_n$, and then project $v$ to be orthogonal to $q_1,\ldots,q_n$ by modified Gram–Schmidt to get $q_{n+1}$.  Crucially, this avoids explicitly computing {x₀,Ax₀,...,Aⁿ⁻¹x₀}, which is a terribly ill-conditioned basis for 𝒦ₙ cannot be post-processed (in finite precision) to obtain an accurate orthogonalization.

**Further reading:** FNC book, [section 8.4 on Krylov subspaces and Arnoldi](https://fncbook.com/subspace). Trefethen lecture 33 on Arnoldi. [This 2009 course](https://web.cs.ucdavis.edu/~bai/Winter09/) on numerical linear algebra by Zhaojun Bai has [useful notes](https://web.cs.ucdavis.edu/~bai/Winter09/krylov.pdf) on Krylov methods, including a discussion of the Rayleigh–Ritz procedure.

## Lecture 6 (April 10)

Discussed what it means to find the "best" solution in the Krylov subspace 𝒦ₙ. Discussed the general principle of Rayleigh–Ritz methods for approximately solving the eigenproblem in a subspace: finding the Ritz vectors/values (= eigenvector/value approximations) with a residual perpendicular to the subspace (a special case of a Galerkin method).

For Hermitian matrices A, I showed that the max/min Ritz values are the maximum/minimum of the Rayleigh quotient in the subspace, via the min–max theorem.  In this sense, at least for Hermitian matrices, the Ritz vectors are *optimal* in the sense of maximizing (or minimizing) the Rayleigh quotient in the Krylov space.  Another sense in which they are optimal for Hermitian A is that the Ritz vectors/values minimize ‖AV-VD‖₂ over all possible orthonormal bases V of the Krylov space and all possible diagonal matrices D (see the Bai notes below for a proof).   (Later, we will discuss an "optimal polynomial" interpretation of Arnoldi+Rayleigh–Ritz from Trefethen lecture 34.)

Moreover, showed that the dot products taken during the Gram–Schmidt process are *exactly* the entries of our Rayleigh–Ritz matrix $H_n = Q_n^T A Q_n$.  This also means that $H_n$ is an [upper-Hessenberg matrix](https://en.wikipedia.org/wiki/Hessenberg_matrix) (*almost* upper triangular), a common intermediate step in many eigensolver algorithms.

Showed that in the case where A is Hermitian, Hₙ is Hermitian as well, so Hₙ is tridiagonal and most of the dot products in the Arnoldi process are zero.  Hence Arnoldi reduces to a three-term recurrence, and the Ritz matrix is tridiagonal.  This is called the **Lanczos** algorithm.

**Further reading**: for Gram–Schmidt, see e.g. Strang Intro to Linear Algebra, chapter 4, and Strang [18.06 lecture 17](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/video-lectures/lecture-17-orthogonal-matrices-and-gram-schmidt).  Modified Gram–Schmidt is analyzed in Trefethen lecture 8, and a detailed analysis with proofs can be found in e.g. [this 2006 paper by Paige et al.](https://epubs.siam.org/doi/abs/10.1137/050630416) \[_SIAM J. Matrix Anal. Appl._ **28**, pp. 264-284\].  See also Per Persson's [18.335 slides on Gram–Schmidt](https://github.com/mitmath/18335/blob/spring21/notes/lec5.pdf).  See also the links on Arnoldi from last lecture.

## Lecture 7 (April 13)

* [Arnoldi experiments in Julia](https://github.com/mitmath/18335/blob/spring21/notes/Arnoldi.ipynb)

Reviewed some experimental results with a very simple implementation of the Arnoldi algorithm (see notebook above).  Arnoldi indeed converges much faster than power iterations, and can give multiple eigenvalues at once.   Like the power method convergence is slower if the desired eigenvalues are clustered closely with undesired ones.  Unlike the power method, it can converge not just to the largest |λ| but to any desired "edge" of the set of eigenvalues (the "spectrum"), e.g. the λ with the most positive or most negative real parts.   Unlike the power method, the convergence of the Arnoldi algorithm is shift-invariant: it is the same for $A$ and $A + \mu I$ for any shift $\mu$.   Like the power method, you can also apply Arnoldi to a shift/invert operator $(A - \mu I)^{-1}$ to find the λ closest to any desired μ, assuming you have a fast way to solve $(A - \mu I)y=x$ for $y$.

Discussed how rounding problems cause a loss of orthogonality in Lanczos, leading to "ghost" eigenvalues where extremal eigenvalues re-appear. In Arnoldi, we explicitly store and orthogonalize against all $q_j$ vectors, but then another problem arises: this becomes more and more expensive as n increases.  In general, the computational cost of $n$  steps Arnoldi with an $m \times m$ matrix is $O(mn^2)$ plus $n$ matrix–vector multiplications, and the storage is $O(mn)$ (for $Q_n$).  Without re-orthogonalization, Lanczos has $O(mn)$ computational cost (+ matvecs), but you still need to store $Q_n$ if you want eigenvectors.  Often, the limiting factor is the $O(mn)$ storage: in linear algebra, we often run out of memory before we run out of time.

A solution to the loss of orthogonality in Lanczos and the growing computational effort in Arnoldi, along with the growing storage, is restarting schemes, where we go for n steps and then restart with the k "best" eigenvectors.   If we restart with k=1 *every* step, then we essentially have the power method, so while restarting makes the convergence worse, the algorithm still converges (at least for the largest |λ| eigenvalues), and converges significantly faster than the power method for n>1.

**Further reading:** Trefethen, lecture 36. See the section on [implicitly restarted Lanczos](http://www.cs.utk.edu/~dongarra/etemplates/node117.html) in [Templates for the Solution of Algebraic Eigenvalue Problems](http://www.cs.utk.edu/~dongarra/etemplates/book.html).  Restarting schemes for Arnoldi (and Lanczos) turn out to be rather subtle — you first need to understand why the most obvious idea (taking the $k$ best Ritz vectors) is *not* a good idea, as [explained in these 18.335 notes](https://github.com/mitmath/18335/blob/spring21/notes/restarting-arnoldi.pdf).

## Lecture 8 (April 15)

* [pset 2 solutions](psets/pset2sol.ipynb)

There are many other eigensolver algorithms besides Arnoldi; the choice of algorithm depends strongly on the properties of the matrix and the desired eigenvalue.  For Hermitian/real-symmetric problems, a powerful algorithm is [LOBPCG](https://en.wikipedia.org/wiki/LOBPCG), a specialized algorithm for minimizing or maximizing the Rayleigh quotient.  There are also a remarkable class of algorithms based on the [residue theorem](https://en.wikipedia.org/wiki/Residue_theorem) of complex analysis, which allow you to efficiently extract all eigenvalues within in a specified region of the complex plane; a prominent version of this is [FEAST](https://www.feast-solver.org/references.htm).

Discussed a simplified version of the LOBPCG in detail.  The basic idea is that for real $A = A^T$ (or more generally complex Hermitian problems), the minimum (most negative) eigenvalue corresponds to minimizing the Rayleigh quotient $R(x) = x^T A x / x^T x$.   In principle, we can apply any gradient-based algorithm to this, such as [gradient-descent methods from machine learning](https://en.wikipedia.org/wiki/Stochastic_gradient_descent), but it turns out that $R(x)$ is so special that we can do *much* better.

* We saw before that $\nabla R = \frac{1}{x^T x} \left[ Ax - R(x) x \right]$, so one could naively apply steepest descent with a fixed "learning rate" $\alpha > 0$, corresponding to iterations $x_{n+1} = x_n - \alpha \left. \nabla R \right|_{x_n}$, requiring one to fiddle with the hyper-parameter $\alpha$.
* One "ideal" choice of $\alpha$ would be the result of a [line search](https://en.wikipedia.org/wiki/Line_search) $\alpha = \arg \min_{\alpha} R(x_n - \alpha \nabla R)$.  It turns out that we can solve this *exactly* by a $2 \times 2$ eigenproblem!   Showed that exact line search is equivalent to $x_{n+1} = Q_2 z$ where $z$ is an eigenvector with the minimum $\lambda$ of the $2 \times 2$ matrix $A_2 = Q_2^T A Q_2$, with $Q_2 = \begin{pmatrix} q_1 & q_2 \end{pmatrix}$ being an orthonormal basis for the span of $\{ x_n, \nabla R \}$.  You could call this a "locally optimal" steepest descent eigensolver.
* A common idea to accelerate gradient descent is a "momentum" or "heavy ball" method, in which $x_{n+1} = x_n - \alpha \left. \nabla R \right|_{x_n} + \beta (x_n - x_{n-1})$ for some hyper-parameter $\beta > 0$: one adds "memory" to the method by remembering the previous step $x_n - x_{n-1}$ and continuing to move somewhat in that direction, inhibiting "zig-zagging" … if $\beta$ is chosen correctly.   For $R(x)$, it turns out that we can minimize $R(x_{n+1})$ by minimizing over *all* $\alpha,\beta$ simply by solving a $3 \times 3$ eigenproblem with $Q_3^T A Q_3$ where  $Q_3$ gives an orthonormal basis for the span of $\{ x_n, \nabla R, x_{n-1} \}$.  Moreover, just as Lanczos is an ideal Krylov method for Hermitian problems that, unlike Arnoldi, only requires you to "remember" one previous step, it turns out that optimizing over the span of $Q_3$ is sufficient to be an ideal Krylov method *asymptotically* as one approaches the minimum of $R(x)$.  Near the minimum, $R(x)$ is approximately quadratic, and the $3 \times 3$ eigenproblem iteration turns out to become equivalent to the [conjugate-gradient method](https://en.wikipedia.org/wiki/Conjugate_gradient_method), an ideal Krylov method for $Ax=b$ that we will discuss soon.   Hence, you could call this a "locally optimal conjugate-gradient" (LOCG) eigensolver.

To go from LOCG to LOBPCG, you need to incorporate two additional tricks.  One is [preconditioning](https://en.wikipedia.org/wiki/Preconditioner), which we will discuss soon.   The other is a "block" version of the Rayleigh quotient which allows you to search for the $k$ smallest (or largest) eigenvalues at once, replacing $x$ with an $m \times k$ matrix $X$.  (Alternatively, you could search for one eigenvalue at a time and use deflation.)

## Lecture 9 (April 17)

* [pset 3](psets/pset3.ipynb)

Introduced the [**GMRES**](https://en.wikipedia.org/wiki/Generalized_minimal_residual_method) algorithm: compute the basis Qₙ for 𝒦ₙ as in Arnoldi, but then minimize the residual ‖Ax-b‖₂ for x∈𝒦ₙ using this basis.  This yields a small (n+1)×n least-squares problem involving Hₙ.

Like Arnoldi/Lanczos, if GMRES does not converge quickly we must generally **restart** it, usually with a subspace of dimension 1; restarting GMRES repeatedly after k steps is called **GMRES(k)**. Unfortunately, unlike Arnoldi for the largest |λ|, restarted GMRES is _not guaranteed to converge_. If it doesn't converge, or if it simply converges slowly, we must do something to speed up convergence: preconditioning.

The solution to this problem is **preconditioning**: finding an (easy-to-compute) M such that MA (left preconditioning) or AM (right preconditioning) has clustered eigenvalues (solving MAx=Mb or AMy=b with x=My, respectively). Essentially, one can think of M as a crude approximation for A⁻¹ (or the inverse of a crude approximation of A that is easy to invert). Brief summary of some preconditioning ideas: multigrid, incomplete LU/Cholesky, Jacobi/block-Jacobi. (Since Jacobi preconditioners only have short-range interactions, they tend not to work well for matrices that come from finite-difference/finite-element discretizations on grids, but they can work well for diagonally dominant matrices that arise in spectral and integral-equation methods.)

To get a more precise understanding of how GMRES (and other Krylov methods) converge, we will transform it to a problem of "polynomial fitting" — it turns out that the error after $n$ steps of GMRES is closely related to the error in "fitting" a degree-$n$ polynomial (with p(0)=1) to the eigenvalues, favoring clustered eigenvalues.

**Further reading (GMRES):** For GMRES, see [FNC section 8.5](https://fncbook.com/gmres) and Trefethen, lectures 35.  A nice plot from GMRES applied to deconvolution can be seen in [this tutorial blog post](https://rikvoorhaar.com/blog/gmres).  A paper proving the existence of matrices where restarted GMRES fails to converge is [Vecharynski and Langou (2009)](https://arxiv.org/abs/0907.3573), and there are even cases where increasing the restart parameter $k$ *worsens* convergence [(Embree, 2003)](https://epubs.siam.org/doi/abs/10.1137/S003614450139961).  Even when it converges, GMRES (and other iterative algorithms) may converge unacceptably slowly without preconditioning.  For Hermitian A, we can also specialize the GMRES algorithm analogous to Lanczos, giving MINRES and SYMMLQ: [Differences in the effects of rounding errors in Krylov solvers for symmetric indefinite linear systems](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.31.3064) by Sleijpen (2000); see also van der Vorst notes from Lecture 22 and the _Templates_ book.


**Further reading (preconditioning):**  [FNC section 8.8](https://fncbook.com/precond) on preconditioning and Trefethen, lecture 40.  [Templates for the Solution of Linear Systems](http://www.netlib.org/linalg/html_templates/Templates.html), chapter on [preconditioners](http://www.netlib.org/linalg/html_templates/node51.html), and e.g. _[Matrix Preconditioning Techniques and Applications](http://books.google.com?id=d9UdanCqJ1QC)_ by Ke Chen (Cambridge Univ. Press, 2005).


## Lecture 10 (April 12)

* [pset 3 solutions](psets/pset3sol.ipynb)
* pset 4: coming soon

To get a more precise understanding of how GMRES (and other Krylov methods) converge, started transforming it to a problem of "polynomial fitting" — it turns out that the error after $n$ steps of GMRES is closely related to the error in "fitting" a degree-$n$ polynomial (with p(0)=1) to the eigenvalues, favoring clustered eigenvalues.

One useful trick that we needed was based on a property of induced norms.  Recall that the induced norm $\Vert A \Vert$ of a matrix $A$ is defined as the maximum possible value of $\Vert A x \Vert / \Vert x \Vert$ (and equals the largest singular value of $A$ in the L2/Euclidean norm).   From this, it immediately follows that $\Vert ABx \Vert \le \Vert A \Vert \cdot \Vert B \Vert \cdot \Vert b \Vert$.   In the GMRES analysis, this allowed us to separate out terms in the diagonalization of $A$.

Some key points:
* GMRES works best if the matrix is well-conditioned and the eigenvalues are mostly in a few tight clusters, similar to the identity matrix.   Preconditioning tries to improve this.
* Because of the p(0)=1 constraint of the GMRES polynomial, convergence of GMRES for $A$ is *not* the same as for a shifted matrix $A + \mu I$.  In particular, as the matrix becomes more ill-conditioned, i.e. one eigenvalue gets close to zero relative to the biggest λ, GMRES convergence slows.
* Arnoldi's analysis is similar (see Trefethen), but its polynomial $p(z)$ has the n-th coefficient equal to 1, rather than the 0-th coefficient.  This makes (unrestarted) Arnoldi *shift-invariant*: it converges equally well for $A$ and $A + \mu I$.
* In Arnoldi, the Ritz values (eigenvalue estimates) are precisely the roots of the optimal polynomial $p(z)$.  This means that Arnoldi works best if the desired eigenvalues are **extremal** (on the edges of the spectrum, e.g. the most positive or most negative real or imaginary parts, or biggest magnitudes) and are **not** clustered with many undesired eigenvalues.    Shift-and-invert $(A - \mu I)^{-1}$ is a way of "exploding" clusters near $\mu$, and for transforming the interior of the spectrum near $\mu$ to the edges of the spectrum.

**Further reading (GMRES, Arnoldi, and polynomials):** Trefethen, lectures 34, 35, 40.   There are also eigenvalue algorithms that can exploit preconditioning if supplied, e.g. the [Jacobi–Davidson algorithm](https://doi.org/10.1002/gamm.201490038) or the LOBPCG algorithm mentioned earlier.  You can construct the Arnoldi polynomial explicitly from its roots, the Ritz values; the analogous construction of the GMRES polynomial uses "harmonic" Ritz values, as explained in e.g. [Goossens and Roose (1999)](https://doi.org/10.1002/(SICI)1099-1506(199906)6:4%3C281::AID-NLA158%3E3.0.CO;2-B).
