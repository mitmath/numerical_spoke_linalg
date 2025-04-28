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
* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0)
* GMRES for Ax=b

Like Arnoldi/Lanczos, if GMRES does not converge quickly we must generally **restart** it, usually with a subspace of dimension 1; restarting GMRES repeatedly after k steps is called **GMRES(k)**. Unfortunately, unlike Arnoldi for the largest |λ|, restarted GMRES is _not guaranteed to converge_. If it doesn't converge, or if it simply converges slowly, we must do something to speed up convergence: preconditioning (next time).

The solution to this problem is **preconditioning**: finding an (easy-to-compute) M such that MA (left preconditioning) or AM (right preconditioning) has clustered eigenvalues (solving MAx=Mb or AMy=b with x=My, respectively). Essentially, one can think of M as a crude approximation for A⁻¹ (or the inverse of a crude approximation of A that is easy to invert). Brief summary of some preconditioning ideas: multigrid, incomplete LU/Cholesky, Jacobi/block-Jacobi. (Since Jacobi preconditioners only have short-range interactions, they tend not to work well for matrices that come from finite-difference/finite-element discretizations on grids, but they can work well for diagonally dominant matrices that arise in spectral and integral-equation methods.)

To get a more precise understanding of how GMRES (and other Krylov methods) converge, started transforming it to a problem of "polynomial fitting" — it turns out that the error after $n$ steps of GMRES is closely related to the error in "fitting" a degree-$n$ polynomial (with p(0)=1) to the eigenvalues, favoring clustered eigenvalues.

One useful trick that we needed was based on a property of induced norms.  Recall that the induced norm $\Vert A \Vert$ of a matrix $A$ is defined as the maximum possible value of $\Vert A x \Vert / \Vert x \Vert$ (and equals the largest singular value of $A$ in the L2/Euclidean norm).   From this, it immediately follows that $\Vert ABx \Vert \le \Vert A \Vert \cdot \Vert B \Vert \cdot \Vert b \Vert$.   In the GMRES analysis, this allowed us to separate out terms in the diagonalization of $A$.

**Further reading (preconditioning):**  [FNC section 8.8](https://fncbook.com/precond) on preconditioning and Trefethen, lecture 40.  [Templates for the Solution of Linear Systems](http://www.netlib.org/linalg/html_templates/Templates.html), chapter on [preconditioners](http://www.netlib.org/linalg/html_templates/node51.html), and e.g. _[Matrix Preconditioning Techniques and Applications](http://books.google.com?id=d9UdanCqJ1QC)_ by Ke Chen (Cambridge Univ. Press, 2005).   For Hermitian A, we can also specialize the GMRES algorithm analogous to Lanczos, giving MINRES and SYMMLQ: [Differences in the effects of rounding errors in Krylov solvers for symmetric indefinite linear systems](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.31.3064) by Sleijpen (2000); see also van der Vorst notes from Lecture 22 and the _Templates_ book.

**Further reading (GMRES and polynomials):** Trefethen, lectures 34, 35. The convergence of GMRES for very non-normal matrices is a complicated subject; see e.g. [this paper on GMRES for defective matrices](http://citeseer.ist.psu.edu/viewdoc/summary?doi=10.1.1.48.1733) or [this paper surveying different convergence estimates](http://eprints.maths.ox.ac.uk/1290/). Regarding convergence problems with GMRES, see this 2002 presentation by Mark Embree on [Restarted GMRES dynamics](http://www.caam.rice.edu/~embree/householder.pdf). [Cullum (1996)](http://link.springer.com/article/10.1007%2FBF02127693) reviews the relationship between GMRES and a similar algorithm called FOM that is more Galerkin-like (analogous to Rayleigh–Ritz rather than least-squares).

## Lecture 11 (April 25)
* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0)
* pset 3 due tomorrow, solutions coming soon
* pset 4: coming soon

Continued discussion of polynomial viewpoint on GMRES and Arnoldi convergence, from last lecture.  Some key points:
* GMRES works best of the eigenvalues are mostly in a cluster, similar to the identity matrix.   Preconditioning tries to improve this.
* Because of the p(0)=1 constraint of the GMRES polynomial, convergence of GMRES for $A$ is *not* the same as for a shifted matrix $A + \mu I$.  In particular, as the matrix becomes more ill-conditioned, i.e. one eigenvalue gets close to zero relative to the biggest λ, GMRES convergence slows.  Preconditioning (as well as other efforts in reformulating the origin of the matrix) tries to make the matrix well-conditioned.
* Arnoldi's analysis is similar (see Trefethen), but its polynomial $p(z)$ has the n-th coefficient equal to 1, rather than the 0-th coefficient.  This makes (unrestarted) Arnoldi *shift-invariant*: it converges equally well for $A$ and $A + \mu I$.
* In Arnoldi, the Ritz values (eigenvalue estimates) are precisely the roots of the optimal polynomial $p(z)$.  This means that Arnoldi works best if the desired eigenvalues are **extremal** (on the edges of the spectrum, e.g. the most positive or most negative real or imaginary parts, or biggest magnitudes) and are **not** clustered with many undesired eigenvalues.    Shift-and-invert $(A - \mu I)^{-1}$ is a way of "exploding" clusters near $\mu$, and for transforming the interior of the spectrum near $\mu$ to the edges of the spectrum.

**Further reading (GMRES, Arnoldi, and polynomials):** Trefethen, lectures 34, 35, 40.   There are also eigenvalue algorithms that can exploit preconditioning if supplied, e.g. the [Jacobi–Davidson algorithm](https://doi.org/10.1002/gamm.201490038) or the LOBPCG algorithm mentioned earlier.  You can construct the Arnoldi polynomial explicitly from its roots, the Ritz values; the analogous construction of the GMRES polynomial uses "harmonic" Ritz values, as explained in e.g. [Goossens and Roose (1999)](https://doi.org/10.1002/(SICI)1099-1506(199906)6:4%3C281::AID-NLA158%3E3.0.CO;2-B).

## Lecture 12 (April 28)
* [Handwritten notes](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0)
* [Julia notebook on gradient descent for quadratic functions](https://nbviewer.org/github/mitmath/18065/blob/main/notes/Quadratic-Gradient-Descent.ipynb)

Introduced the problem of minimizing **convex quadratic** functions $f(x) = \frac{1}{2} x^T A x - b^T x$ where $A = A^T$ is [symmetric positive definite (SPD)](https://en.wikipedia.org/wiki/Definite_matrix); this is also known as unconstrained [quadratic programming](https://en.wikipedia.org/wiki/Quadratic_programming).  Such a "bowl"-shaped function has a single minimum.  By taking the gradient, we find that the "downhill" direction is $-\nabla f = b - Ax = r$, where $r$ is the **residual**, immediately telling us that the solution is the $x$ solving $Ax=b$.  That is, minimizing a convex quadratic is equivalent to solving SPD systems.  This means that **optimization algorithms can be used to solve SPD Ax=b**, and conversely that better ways of solving SPD systems give us better ways to do quadratic optimization.  This is important because:

1. SPD matrices show up in lots of real-world problems, any time $A = B^T B$ for some $B$ with independent columns.  This includes physics and graph problems involving the Laplacian operator $-\nabla^2$, which discretizes into an SPD matrix (assuming appropriate boundary conditions), as well as in least-square problems (which are convex quadratics).
2. *Any* smooth $f(x)$ is **approximately convex quadratic** near a local minimum (unconstrained): just think of the Taylor series around a minimum where $\nabla f = 0$.   This means that understanding how to solve SPD systems quickly can potentially help us accelerate nearly *any* optimization problem.

Introduced the method of **steepest descent** or simply **gradient descent**: at each step $k$, given a current estimate $x_k$ for the solution, make an improved estimate $x_{k+1} = x_k + \alpha r_k$, where $r_k = b - Ax_k = - \left. \nabla f \right_{x_k}$ is the current "downhill" gradient direction.   This is a very general (albeit primitive) strategy, the starting point for many optimization algorithms on very general functions.   How do we choose the stepsize parameter $\alpha$ (called the "learning rate" in ML)?  For an arbitrary nonlinear function $f(x)$, this is a tricky problem, but for the specific case of a quadratic $f$ we can solve for an "optimal" α analytically by "exact line search".

In particular, suppose we are taking a step $x_{k+1} = x_k + \alpha d$ in some direction $d$ (e.g. $d=r_k$).  It is natural to choose the $\alpha$ that *minimizes* $f(x_{k+1})$.  We can solve this by setting the $\partial/\partial \alpha$ derivative to zero, but we can also see "geometrically" that the minimum must satisfy $d^T r_{k+1} = 0$: at the new point $x_{k+1}$, the gradient $-r_{k+1}$ must be $\perp d$, as otherwise it wouldn't minimize the value along the line.   Hence, we can show that $\alpha = d^T r_k / d^T A d$.  For $d = r_k \ne 0$, this implies $\alpha >0$: intuitively, we will always take a positive step in the downhill direction.

However, showed with a numerical example (see [notebook](https://www.dropbox.com/scl/fi/pxea51ooxryw2fo4t3rt6/Large-scale-Linalg-Spring-2025.pdf?rlkey=kbekxxgyp8xovp55nnsvrrxds&st=k76yqpnw&dl=0)) that **gradient descent tends to "zig-zag"**, often converging rather slowly.  The basic problem is if $A$ has some eigenvalues much large than others, the function $f(x)$ looks like a long valley with steep walls, and the downhill direction points mostly perpendicular to the steep walls, rather than *along* the valley towards the optimum.  We want to improve this by deriving a Krylov-subspace method that minimizes f(x) over all previous search directions simultaneously.

**Further reading:** Strang *Linear Algebra and Learning from Data* section VI.4; 18.065 [OCW lecture 21: Minimizing a Function Step by Step](https://ocw.mit.edu/courses/18-065-matrix-methods-in-data-analysis-signal-processing-and-machine-learning-spring-2018/resources/lecture-21-minimizing-a-function-step-by-step/) and [OCW lecture 22: Gradient Descent — Downhill to a Minimum](https://ocw.mit.edu/courses/18-065-matrix-methods-in-data-analysis-signal-processing-and-machine-learning-spring-2018/resources/lecture-22-gradient-descent-downhill-to-a-minimum/).   See also the useful notes, [An introduction to the conjugate gradient method without the agonizing pain](http://www.cs.cmu.edu/~quake-papers/painless-conjugate-gradient.pdf) by J. R. Shewchuk.

## Lecture 13 (April 30)
Convergence of gradient descent.  Conjugate gradients.

## Lecture 14 (May 2)
* Conjugate gradient, continued.
* pset 4 due, solutions

## Lecture 15 (May 5)
* Other iterative algorithms: Overview

## Lecture 16 (May 7)
* Randomized linear algebra: the randomized SVD and low-rank approximation

## Lecture 17 (May 9)
* Differentiating linear algebra solutions: Adjoint methods

## Lecture 18 (May 12)
* final projects due
