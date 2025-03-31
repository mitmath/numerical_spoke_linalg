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

**Resources**: Piazza discussion forum (TO BE CREATED), [math learning center](https://math.mit.edu/learningcenter/), [TSR^2 study/resource room](https://ome.mit.edu/programs/talented-scholars-resource-room-tsr2), [pset partners](https://psetpartners.mit.edu/).

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

## Lecture 3 (April 4)
* pset 1 posted

* Sparse-direct algorithms: nested dissection

## Lecture 4 (April 7)

* Iterative methods: the big picture.
* Power method for eigenvalues: Convergence rate.  Deflation.

## Lecture 5 (April 9)

* Rayleigh quotients, inverse and shifted-inverse iteration.

## Lecture 6 (April 11)
* Better than the power method: Krylov subspace methods and Arnoldi.
* pset 1 due, solutions
* pset 2 posted

## Lecture 7 (April 14)
* Arnoldi and Rayleigh–Ritz estimation of eigenpairs from a Krylov basis.

## Lecture 8 (April 16)
* Lanczos
* Restarting

## Lecture 9 (April 18)
* GMRES for Ax=b
* project proposals due
* pset 2 due, solutions
* pset 3 posted

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
