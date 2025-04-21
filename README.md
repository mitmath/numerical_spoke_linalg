# Interdisciplinary Numerical Methods: Linear-Algebra "Spoke" 18.S097/16.S092

This repository holds materials the second 6-unit "spoke" half of a new MIT course (**Spring 2025**) introducing numerical methods and numerical analysis to a broad audience.   18.S097/16.S092 covers **large-scale linear algebra**: what do you do when the matrices get so huge that you probably can't even store them as $n \times n$ arrays, much less run $O(n^3)$ algorithms like ordinary Gaussian elimination?

* **Prerequisites:** 18.03, 18.06, or equivalents, and some programming experience.   You should have taken the [first half-semester numerical "hub" 18.S190/16.S090](https://github.com/mitmath/numerical_hub), or alternatively *any other introductory numerical-methods course* (e.g. 18.330, 18.C25, 18.C20, 16.90, 2.086, 12.010, 6.7330J/16.920J/2.097J, or 6.S955).

Taking both the hub and any spoke will count as an 18.3xx class for math majors, similar to 18.330, and as 16.90 for course-16 majors.

**Instructor**: [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj).

**Lectures**: MWF10 in 2-142 (Feb 3 – Mar 31), slides and notes posted below.  Lecture videos posted in [Panopto Video on Canvas](https://canvas.mit.edu/courses/32076/external_tools/595).

**Grading** (all assignments **submitted electronically** via [Gradescope on Canvas](https://canvas.mit.edu/courses/32076/external_tools/369)):
* 50% **4 weekly psets:** due Fridays at midnight: April 11, 18, 25 and May 2.
* 50% **final project**: due May 12 (last day of class). The final project will be an 8–15 page paper reviewing, implementing, and testing some interesting numerical linear-algebra algorithm not covered in the course.  A 1-page *proposal* will be due April 18.  See [final-project/proposal information](psets/proposal.md).

**Collaboration policy:** Talk to anyone you want to and read anything you want to, with two caveats: First, make a solid effort to solve a problem on your own before discussing it with classmates or googling. Second, no matter whom you talk to or what you read, write up the solution on your own, without having their answer in front of you (this includes ChatGPT and similar). (You can use [psetpartners.mit.edu](https://psetpartners.mit.edu/) to find problem-set partners.)

**Teaching Assistant**: [Mo Chen](https://math.mit.edu/directory/profile.html?pid=2176)

**Office Hours**: Wednesday 4pm in 2-345 (Prof. Johnson)

**Resources**: [Piazza discussion forum](https://piazza.com/mit/spring2025/18s097), [math learning center](https://math.mit.edu/learningcenter/), [TSR^2 study/resource room](https://ome.mit.edu/programs/talented-scholars-resource-room-tsr2), [pset partners](https://psetpartners.mit.edu/).

**Textbook**: No required textbook, but suggestions for further reading will be posted after each lecture.  The book [*Fundamentals of Numerical Computation* (FNC)](https://fncbook.com/) by Driscoll and Braun is **freely available online**, has examples in Julia, Python, and Matlab, and is a valuable resource.   Another important textbook for the course is [_Numerical Linear Algebra_ by Trefethen and Bau](http://www.amazon.com/Numerical-Linear-Algebra-Lloyd-Trefethen/dp/0898713617). ([Readable online](http://owens.mit.edu/sfx_local?bookid=9436&rft.genre=book&sid=Barton:Books24x7) with MIT certificates, and there is [also a PDF posted online at uchicago](https://www.stat.uchicago.edu/~lekheng/courses/309/books/Trefethen-Bau.pdf), though this is a graduate-level textbook and hence is somewhat more sophisticated than the coverage in this course.)

This document is a *brief* summary of what was covered in each
lecture, along with links and suggestions for further reading.  It is
*not* a good substitute for attending lecture, but may provide a
useful study guide.

## Lecture 1 (March 31)

* Overview and syllabus (this web page).
* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0)
* [Julia notebook with scaling examples](notes/linalg-scaling.ipynb)
* Lecture video (MIT only): [panopto video on canvas](https://canvas.mit.edu/courses/32076/external_tools/595)

Reviewed the fact that traditional "dense" linear-algebra algorthms (factorizations LU, QR, diagonalization, SVD, etc.), which assume little or no special structure of the matrix, typically require $O(m^3)$ cost and $O(m^2)$ memory.   In practice, this means that $m=1000$ matrices are easy laptop-scale problems, but much bigger matrices like $m=10^6$ seem out of reach.

However, huge matrices ($m=10^6$ or more) commonly arise in many applications, such ans engineering models of partial differential equations (where functions are discretized onto a grid or mesh).   A $100 \times 100 \times 100$ mesh is a *small* model of a 3d system, and that has $10^6$ unknowns.   How do people handle such problems?

The trick is that huge matrices typically have some special structure that you can exploit, and the most common such structure is **[sparsity](https://en.wikipedia.org/wiki/Sparse_matrix): the matrix entries are mostly zero**.  Ideally, an $m \times m$ sparse matrix should only have $O(m)$ nonzero entries.

Showed how a sparse matrix, in fact a symmetric [tridiagonal matrix](https://en.wikipedia.org/wiki/Tridiagonal_matrix) arises from discretizing a simple PDE on a grid with finite differences: [Poisson's equation](https://en.wikipedia.org/wiki/Poisson%27s_equation) $d^2 u / dx^2 = f$.   (Sparsity arises because differential operators like $\nabla^2$ are *local* operators in space, so on a grid or mesh they only couple neighboring elements.)   Computationally, we can store and solve such a matrix *very* efficiently, with $O(m)$ work.

How can we generalize this to other sparsity patterns and other types of large-scale matrix structures?

**Further reading**: FNC book [section 8.1: sparsity and structure](https://fncbook.com/structure-1).  The example of discretizing a 1d Poisson equation with finite differences, resulting in a tridiagonal matrix, is discussed in many sources, e.g. [these UTexas course notes](https://farside.ph.utexas.edu/teaching/329/lectures/node62.html).

## Lecture 2 (April 2)

* Sparse matrices and data structures
* [sparse-matrix slides from 18.335 (Fall 2006)](https://github.com/mitmath/18335/blob/spring21/notes/lec21handout6pp.pdf)
* [Julia notebook on dense and sparse matrices](https://nbviewer.org/github/mitmath/1806/blob/fall22/notes/Dense-and-Sparse.ipynb) from 18.06 (Fall 2022)

## Lecture 3 (April 4)
* [sparse factorization and nested-dissection examples](https://nbviewer.org/github/mitmath/numerical_spoke_linalg/blob/main/notes/Nested-Dissection.ipynb)
* [pset 1](psets/pset1.ipynb): due Friday, April 11

**Sparse-direct solvers:** For many problems, there is an intermediate between the dense Θ(m³) solvers of LAPACK and iterative algorithms: for a sparse matrix A, we can sometimes perform an LU or Cholesky factorization while maintaining sparsity, storing and computing only nonzero entries for vast savings in storage and work.  One key observation is that the fill-in only depends on the pattern of the matrix, which can be interpreted as a [graph](http://en.wikipedia.org/wiki/Graph_%28mathematics%29): m vertices, and edges for the nonzero entries of A (an [adjacency matrix](http://en.wikipedia.org/wiki/Adjacency_matrix) of the graph), and sparse-direct algorithms are closely related to graph-theory problems.    How efficient the sparse-direct methods are depends on how easy it is to **partition** the graph by chopping it into pieces, and this is **easier for matrices that come from low-dimensional meshes** (e.g. discretized low-dimensional PDEs).  1d meshes are best (giving *banded* matrices with *linear* complexity), 2d meshes are still pretty good, and 3d meshes start to become challenging.  See the scalings in the handout, which are derived in the Davis book below.

**Further reading:** The book _[Direct Methods for Sparse Linear Systems](http://books.google.com/books?id=TvwiyF8vy3EC&lpg=PR1&ots=odauEC2c4k&dq=direct%20methods%20for%20sparse%20davis&pg=PR1#v=onepage&q&f=false)_ by Davis is a useful reference.

## Lecture 4 (April 7)

Iterative methods: the big picture is to solve $Ax=b$, $Ax = \lambda x$ and similar methods by iteratively improving a "guess" (usually starting with $x=0$ or random numbers), where the matrix $A$ is *only* used via matrix–vector products $Av$.

* **Pro:** can be fast whenever $Av$ is fast (e.g. if $A$ is sparse, low-rank, [Toeplitz](https://en.wikipedia.org/wiki/Toeplitz_matrix), etc.).  Can scale to huge probems.
* **Con:** hard to design an iteration that converges quickly, how well the methods work is often problem-dependent, often requires problem-depending tuning and experimentation (e.g. [preconditioners](https://en.wikipedia.org/wiki/Preconditioner))

The simplest iterative method is the [power method for eigenvalues](https://en.wikipedia.org/wiki/Power_iteration): repeatedly multipling a random initial vector by $A$ converges to an eigenvector of the largest $|\lambda|$, *if* there is no other eigenvalue with equal magnitude.  Showed an example (easiest for real-symmetric $A=A^T$ matrices).

Analyzed the convergence of the power method: if $\lambda_1$ is the biggest-magnitude eigenvalue and $\lambda_2$ is the second-biggest, then the error in the eigenvector on the k-th iteration is $O(|\lambda_2/\lambda_1|^k)$.

Given an estimate $x$ for an eigenvector, a nice way to get an estimate for the corresponding eigenvalue is the [Rayleigh quotient](https://en.wikipedia.org/wiki/Rayleigh_quotient) $\lambda \approx x^T A x / x^T x$.   Numerically, showed that the error in this eigenvalue estimate is the *square* of the error in $x$ for real-symmetric $A$, and in fact the eigenvalue error converges as $O(|\lambda_2/\lambda_1|^{2k})$ in this case.  (We will see *why* this happens next time.)

To find *other* eigenvectors and eigenvalues, one possibility is an algorithm called **deflation**. It exploits the fact that for real-symmetric $A$, the eigenvectors $q_1, q_2, \ldots$ for distinct $\lambda$ are orthogonal.   So, once we have found $q_1$, we can repeat the power method but **project each step to be orthogonal** to the previously found eigenvector, i.e. replace $x \longleftarrow x - q_1 (q_1^T x)$.  This will then converge to $q_2$ (for the second-biggest $|\lambda|$).  To get $q_3$, repeat the power method but project orthogonal to both $q_1, q_2$ with $x \longleftarrow x - q_1 (q_1^T x) - q_2 (q_2^T x)$, etcetera.

Deflation is a terrible scheme if you want the *smallest* magnitude eigenvalue, however, since you'd have to compute *all* the other eigenvalues/vectors first.   Instead, to find the smallest |λ| one can simply apply the power method to $A^{-1}$: on each step, compute $y = A^{-1} x$, i.e. solve $Ay = x$.  This is called [(unshifted) inverse iteration](https://en.wikipedia.org/wiki/Inverse_iteration).   It relies on a fast way to solve $Ay = x$; for example, if $A$ is sparse, you can compute the sparse LU factorization and re-use it on each step with a sparse-direct solve.   In general, we will see that solution methods for eigenproblems and linear systems are often closely related!

**Further reading:** FNC book [section 8.2: power iteration](https://fncbook.com/power) and [section 8.3: inverse iteration](https://fncbook.com/inviter). Trefethen & Bau, lecture 27.  See [this notebook on the power method](https://nbviewer.org/github/mitmath/1806/blob/fall22/notes/Power-Method.ipynb) from 18.06.

## Lecture 5 (April 9)

Proved that, for a real-symmetric matrix A=Aᵀ, the [Rayleigh quotient R(x)=xᵀAx/xᵀx](https://en.wikipedia.org/wiki/Rayleigh_quotient) is bounded above and below by the largest and smallest eigenvalues of A (the ["min–max theorem"](https://en.wikipedia.org/wiki/Min-max_theorem)).   This theorem is useful for lots of things in linear algebra.  Here, it helps us understand why the Rayleigh quotient is so accurate: the power method converges to a maximum-|λ| eigenvalue, which is either the smallest (most negative) or the largest (most positive) λ of a real-symmetric A, and hence that λ is an *extremum* (minimum or maximum) of the Rayleigh quotient where its gradient is zero.  In fact, you can show that ∇R=0 for *any* eigenvector (not necessarily min/max λ).   This means, if we Taylor expand R(x+δx) around an eigenvector x where R(x)=λ, you get R(x+δx)=λ+O(‖δx‖^2): the error in the eigenvalue λ goes as the *square* of the error in the eigenvector (for real-symmetric A).

Last time, we considered inverse iteration.  A more general idea is **shifted inverse iteration**: at each step, compute $(A - \mu I)^{-1}$ times the previous step, for some shift $\mu$.   This converges to an eigenvector of the λ **closest to μ**.   If μ is very close to an eigenvalue, it converges extremely quickly.   Not only does this allow you to search for eigenvalues anywhere in the spectrum that you want, but it also gives you a way to accelerate iteration if you have a good guess for λ.

But where would you get a good guess for λ?  A simple answer is to use the Rayleigh quotient R(x), where x comes from previous steps of the power iteration.  Even if the power iteration is converging slowly, once you have even a rough approximation for λ you can use it as a shift.  This leads to the algorithm of [Rayleigh-quotient iteration](https://en.wikipedia.org/wiki/Rayleigh_quotient_iteration): at each step, compute $x_k = (A - \mu_k I)^{-1} x_{k-1} / \Vert \cdots \Vert$, where $\mu_k = R(x_{k-1})$.   It turns out that this converges *faster* than exponentially with $k$: it *cubes the error* (triples the number of digits) at every step, once you get close enough to the eigenvalue.  (This is even faster than the quadratic convergence of Newton's method!)

The only problem with Rayleigh-quotient is the need for a good initial guess — if you have a bad initial guess, it can be quite unpredictable what eigenvalue it converges to!   But any time you can obtain a rough idea of where the desired eigenvalue is, it means you can zoom into the exact value extremely quickly.

**Further reading:** FNC book [section 8.3: inverse iteration](https://fncbook.com/inviter); however, beware that the book currently shows a less accurate (for real-symmetric/Hermitian A) method to estimate eigenvalues (issue [fnc#16](https://github.com/fncbook/fnc/issues/16)).  [Trefethen & Bau, lecture 27](https://www.cs.cmu.edu/afs/cs/academic/class/15859n-f16/Handouts/TrefethenBau/RayleighQuotient-27.pdf) covers these algorithms in much more depth.  [These slides by Per Persson (2006)](https://github.com/mitmath/18335/blob/spring21/notes/lec15handout6pp.pdf) are a useful summary.


## Lecture 6 (April 11)
* [pset 1 solutions](psets/pset1sol.ipynb)
* [pset 2](psets/pset2.ipynb), due April 18

Introduced Krylov subspaces, and the idea of **Krylov subspace** methods: ideally, we want to find the "best" solution in the *whole subspace* 𝒦ₙ spanned by {x₀,Ax₀,...,Aⁿ⁻¹x₀}, which is the *only* subspace you can get starting from x₀ if you are only allowed linear operations and matrix–vector products.

The **Arnoldi** algorithm is a Krylov algorithm for eigenproblems.  It basically has two components:

1. Find an orthonormal basis Qₙ for 𝒦ₙ.   Essentially, we will to this by a form of Gram–Schmidt, to be determined.
2. Given the basis, give the "best" estimate in 𝒦ₙ for one or more eigenvectors and eigenvalues.

Discussed what it means to find the "best" solution in the Krylov subspace 𝒦ₙ. Discussed the general principle of Rayleigh–Ritz methods for approximately solving the eigenproblem in a subspace: finding the Ritz vectors/values (= eigenvector/value approximations) with a residual perpendicular to the subspace (a special case of a Galerkin method).

For Hermitian matrices A, I showed that the max/min Ritz values are the maximum/minimum of the Rayleigh quotient in the subspace, via the min–max theorem.  In this sense, at least for Hermitian matrices, the Ritz vectors are *optimal* in the sense of maximizing (or minimizing) the Rayleigh quotient in the Krylov space.  Another sense in which they are optimal for Hermitian A is that the Ritz vectors/values minimize ‖AV-VD‖₂ over all possible orthonormal bases V of the Krylov space and all possible diagonal matrices D (see the Bai notes below for a proof).   (Later, we will discuss an "optimal polynomial" interpretation of Arnoldi+Rayleigh–Ritz from Trefethen lecture 34.)

**Further reading:** FNC book, [section 8.4 on Krylov subspaces and Arnoldi](https://fncbook.com/subspace). Trefethen lecture 33 on Arnoldi. [This 2009 course](https://web.cs.ucdavis.edu/~bai/Winter09/) on numerical linear algebra by Zhaojun Bai has [useful notes](https://web.cs.ucdavis.edu/~bai/Winter09/krylov.pdf) on Krylov methods, including a discussion of the Rayleigh–Ritz procedure.

## Lecture 7 (April 14)
* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0)
* [Arnoldi experiments in Julia](https://nbviewer.org/github/mitmath/18335/blob/spring21/notes/Arnoldi.ipynb)

How do we construct the orthonormal basis $Q_n$ of the Krylov space?  Reviewed the [Gram–Schmidt](https://en.wikipedia.org/wiki/Gram%E2%80%93Schmidt_process) algorithm, along with its numerically stable cousin, modified Gram–Schmidt.  Described how a variation on this idea can be used for the Krylov space: at each step, take the most recent orthonormal basis vector $q_n$, multiply it by $A$ to get $v = Aq_n$, and then project $v$ to be orthogonal to $q_1,\ldots,q_n$ by modified Gram–Schmidt to get $q_{n+1}$.  Crucially, this avoids explicitly computing {x₀,Ax₀,...,Aⁿ⁻¹x₀}, which is a terribly ill-conditioned basis for 𝒦ₙ cannot be post-processed (in finite precision) to obtain an accurate orthogonalization.

Moreover, showed that the dot products taken during the Gram–Schmidt process are *exactly* the entries of our Rayleigh–Ritz matrix $H_n = Q_n^T A Q_n$.  This also means that $H_n$ is an [upper-Hessenberg matrix](https://en.wikipedia.org/wiki/Hessenberg_matrix) (*almost* upper triangular), a common intermediate step in many eigensolver algorithms.

Closed by showing some experimental results with a very simple implementation of the Arnoldi algorithm (see notebook above).  Arnoldi indeed converges much faster than power iterations, and can give multiple eigenvalues at once.   Like the power method convergence is slower if the desired eigenvalues are clustered closely with undesired ones.  Unlike the power method, it can converge not just to the largest |λ| but to any desired "edge" of the set of eigenvalues (the "spectrum"), e.g. the λ with the most positive or most negative real parts.   Unlike the power method, the convergence of the Arnoldi algorithm is shift-invariant: it is the same for $A$ and $A + \mu I$ for any shift $\mu$.   Like the power method, you can also apply Arnoldi to a shift/invert operator $(A - \mu I)^{-1}$ to find the λ closest to any desired μ, assuming you have a fast way to solve $(A - \mu I)y=x$ for $y$.

**Further reading**: for Gram–Schmidt, see e.g. Strang Intro to Linear Algebra, chapter 4, and Strang [18.06 lecture 17](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/video-lectures/lecture-17-orthogonal-matrices-and-gram-schmidt).  Modified Gram–Schmidt is analyzed in Trefethen lecture 8, and a detailed analysis with proofs can be found in e.g. [this 2006 paper by Paige et al.](https://epubs.siam.org/doi/abs/10.1137/050630416) \[_SIAM J. Matrix Anal. Appl._ **28**, pp. 264-284\].  See also Per Persson's [18.335 slides on Gram–Schmidt](https://github.com/mitmath/18335/blob/spring21/notes/lec5.pdf).  See also the links on Arnoldi from last lecture.

## Lecture 8 (April 16)
* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0)
* [Arnoldi experiments in Julia](https://nbviewer.org/github/mitmath/18335/blob/spring21/notes/Arnoldi.ipynb)

Showed that in the case where A is Hermitian, Hₙ is Hermitian as well, so Hₙ is tridiagonal and most of the dot products in the Arnoldi process are zero.  Hence Arnoldi reduces to a three-term recurrence, and the Ritz matrix is tridiagonal.  This is called the **Lanczos** algorithm.

Showed some computational examples (notebook above) of Arnoldi convergence. Discussed how rounding problems cause a loss of orthogonality in Lanczos, leading to "ghost" eigenvalues where extremal eigenvalues re-appear. In Arnoldi, we explicitly store and orthogonalize against all $q_j$ vectors, but then another problem arises: this becomes more and more expensive as n increases.  In general, the computational cost of $n$  steps Arnoldi with an $m \times m$ matrix is $O(mn^2)$ plus $n$ matrix–vector multiplications, and the storage is $O(mn)$ (for $Q_n$).  Without re-orthogonalization, Lanczos has $O(mn)$ computational cost (+ matvecs), but you still need to store $Q_n$ if you want eigenvectors.  Often, the limiting factor is the $O(mn)$ storage: in linear algebra, we often run out of memory before we run out of time.

A solution to the loss of orthogonality in Lanczos and the growing computational effort in Arnoldi, along with the growing storage, is restarting schemes, where we go for n steps and then restart with the k "best" eigenvectors.   If we restart with k=1 *every* step, then we essentially have the power method, so while restarting makes the convergence worse the algorithm still converges, and converges significantly faster than the power method for n>1.

**Further reading:** Trefethen, lecture 36. See the section on [implicitly restarted Lanczos](http://www.cs.utk.edu/~dongarra/etemplates/node117.html) in [Templates for the Solution of Algebraic Eigenvalue Problems](http://www.cs.utk.edu/~dongarra/etemplates/book.html).  Restarting schemes for Arnoldi (and Lanczos) turn out to be rather subtle — you first need to understand why the most obvious idea (taking the $k$ best Ritz vectors) is *not* a good idea, as [explained in these 18.335 notes](https://github.com/mitmath/18335/blob/spring21/notes/restarting-arnoldi.pdf).

## Lecture 9 (April 18)
* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0)
* GMRES for Ax=b
* project proposals due
* [pset 2 solutions](psets/pset2sol.ipynb)
* [pset 3](psets/pset3.ipynb): due Saturday 4/26 at midnight

There are many other eigensolver algorithms besides Arnoldi; the choice of algorithm depends strongly on the properties of the matrix and the desired eigenvalue.  For Hermitian/real-symmetric problems, a powerful algorithm is [LOBPCG](https://en.wikipedia.org/wiki/LOBPCG), a specialized algorithm for minimizing or maximizing the Rayleigh quotient.  There are also a remarkable class of algorithms based on the [residue theorem](https://en.wikipedia.org/wiki/Residue_theorem) of complex analysis, which allow you to efficiently extract all eigenvalues within in a specified region of the complex plane; a prominent version of this is [FEAST](https://www.feast-solver.org/references.htm).

Introduced the [**GMRES**](https://en.wikipedia.org/wiki/Generalized_minimal_residual_method) algorithm: compute the basis Qₙ for 𝒦ₙ as in Arnoldi, but then minimize the residual ‖Ax-b‖₂ for x∈𝒦ₙ using this basis.  This yields a small (n+1)×n least-squares problem involving Hₙ.

**Further reading:** The book [*Templates for the Solution of Algebraic Eigenvalue Problems* (2000)](https://www.cs.ucdavis.edu/~bai/ET/contents.html) gives a number of methods for various types of eigenproblems, but active research in this area continues.  For GMRES, see [FNC section 8.5](https://fncbook.com/gmres) and Trefethen, lectures 35.  In class, I showed a plot from GMRES applied to deconvolution, from [this tutorial blog post](https://rikvoorhaar.com/blog/gmres).

## Holiday (April 21)

## Lecture 10 (April 22)
* Preconditioning

## Lecture 11 (April 25)
* From steepest descent to conjugate gradients.
* pset 3 due, solutions
* pset 4 posted

## Lecture 12 (May 2)
* Conjugate gradient, continued.
* pset 4 due, solutions

## Lecture 13 (May 5)
* Other iterative algorithms: Overview

## Lecture 14 (May 7)
* Randomized linear algebra: the randomized SVD and low-rank approximation

## Lecture 15 (May 9)
* Low-rank updates

## Lecture 16 (May 12)
* Differentiating linear algebra solutions: Adjoint methods
* final projects due
