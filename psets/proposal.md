# 18.335 final projects

The final project will be an 8–15 page paper (single-column, single-spaced, ideally using the style template from the [_SIAM Journal on Numerical Analysis_](http://www.siam.org/journals/auth-info.php)), reviewing some interesting linear-algebra algorithm not covered in the course.  Your paper should be written for an audience of your peers in the class, and should include example numerical results (by you) from application to a realistic problem (small-scale is fine), discussion of accuracy and performance characteristics (both theoretical and experimental), and ideally a comparison to a competing algorithm for the same problem.

Like any review paper, you should _thoroughly reference_ the published literature (citing both original articles and authoritative reviews/books where appropriate \[rarely web pages\]), tracing the historical development of the ideas and giving the reader pointers on where to go for more information and related work and later refinements, with references cited throughout the text (enough to make it clear what references go with what results). (**Note:** you may re-use diagrams from other sources, but all such usage must be _explicitly credited_; not doing so is [plagiarism](http://writing.mit.edu/wcc/avoidingplagiarism).) Model your paper on academic review articles (e.g. read _SIAM Review_ and similar journals for examples).

A good final project will include:

* An extensive introduction and bibliography putting the algorithm in context.  Where did it come from, and what motivated its development?  Where is it commonly used (if anywhere)?  What are the main competing algorithms?  Were any variants of the algorithm proposed later?  What do other authors say about it?

* A clear description of the algorithm, with a summary of its derivation and key properties.   Don't copy long mathematical derivations or proofs from other sources, but do *summarize* the key ideas and results in the literature.

* A convincing validation on a representative/quasi-realistic test problem (i.e. show that your code works).  Performance experiments showing the scaling.

    - Note that this should be your *own* independent re-implementation of the algorithm (in any language of your choice) based on the mathematical description in the paper(s).  (Not based on other people's code.)  The point is to demonstrate that you understand the algorithm well enough to implement it correctly, and that you understand how to make convincing validations and performance tests.

    - Ideally, you should also include an informative comparison to one or more "competing" algorithms for the same problem (someone else's code is okay for this).  For someone who is thinking about using the algorithm, you should strive to give them *useful* guidance on how the algorithm compares to competing algorithms: when/where should you consider using it (if ever)?   Be cautious about comparing via actual timing results — see below!

## What to submit

You should submit:

1. A final-project proposal late in the first month of the quarter, to get feedback on your project topic and other plans.

2. A PDF of your paper (in [SIAM format](http://www.siam.org/journals/auth-info.php) or similar, see above), due on the last day of the term.

3. A `.zip` or `.tar.gz`/`.tgz` archive containing your source code (in whatever programming language).   A `README` file briefly outlining what is in each file would also be appreciated.  (You don't need to include software packages downloaded from elsewhere and used unmodified, but a note in the `README` about software requirements would be good.)

## Frequently asked questions

Frequently asked questions about the final project:

1.  _How formal is the proposal?_ Very informal—one page describing what you plan to do, with a couple of references that you are using as starting points. Basically, the proposal is just so that I can verify that what you are planning is reasonable and to give you some early feedback.
2.  _How much code do I need to write?_ A project should include a working proof-of-concept implementation, e.g. in Julia or Python or Matlab, that *you* wrote to demonstrate that you understand how the algorithm works. Your code does _not_ have to be competitive with "serious" implementations, and I encourage you to download and try out existing "serious" implementations (where available) for any large-scale testing and comparisons.
5.  _How should I do performance comparisons?_ Be very cautious about timing measurements: unless you are measuring highly optimized code or only care about orders of magnitude, timing measurements are more about implementation quality than algorithms. It's often less problematic to *measure something implementation-independent* (like flop counts, or matrix-vector multiplies for iterative algorithms), even though such measures have their own weaknesses.

## Proposal

During the semester you will submit a 1–2 page **proposal** for your intended project.  This is just intendeded so that I can give you early feedback on your plans.  Note in particular that I do **not** expect "research" projects; your project should mostly consist of *known* results that you review, re-implement, validate, compare, and otherwise synthesize.

Your proposal should include:

* A *brief* (1–2 paragraph) description of the algorithm and the problem it solves.
* How you intend to *validate* your implementation (e.g. test problems).
* Information on your planned implementation — language, useful libraries you will exploit?
* What algorithm(s) you intend to compare your implementation to. How will you compare algorithms?
* A few references that you will use as starting points.

## Project topics

As described above you have broad flexibility in choosing a project.  Some key constraints are:

* Must be about "numerical linear algebra": solving linear equations, eigenproblems, or similar.

* Must not be covered in class.  (I will also give you feedback if you propose a topic that I plan to cover in class; usually it will be possible to slightly adjust the focus to a related topic I won't cover.)

A few examples of project topics I've seen students use in related courses is:

* Implicit restarting algorithms for Arnoldi or Lanczos.
* Simultaneous diagonalization of commuting matrices (e.g. [this method](https://doi.org/10.1137/0614062))
* Nonlinear eigenproblems (e.g. contour-integration methods like [FEAST](http://www.ecs.umass.edu/~polizzi/feast/) or other methods like in [NEP-PACK](https://nep-pack.github.io/NonlinearEigenproblems.jl/)).
* Iterative linear-algebra algorithms not covered in class, e.g. BiCGSTAB(ℓ), DQGMRES, or the Jacobi-Davidson eigenproblem algorithm.
* Matrix exponentials: computing the matrix eᴬ, or maybe just y=eᴬx iteratively where A is sparse (e.g. Krylov methods).
* Randomized acceleration of linear solvers, e.g. ["sketched GMRES"](https://arxiv.org/pdf/2111.00113.pdf).

(Try leafing through a book on numerical linear algebra, e.g. the book [Templates for the Solution of Linear Systems](https://www.netlib.org/templates/templates.pdf) or [Templates for the Solution of Algebraic Eigenvalue Problems](https://www.netlib.org/utk/people/JackDongarra/etemplates/book.html).)
