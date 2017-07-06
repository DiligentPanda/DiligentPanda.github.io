---
title: Traditional Methods
date: 2017-06-26 13:14:28
categories: [Programming/AI/Search/How to Solve it]
tags: [book,AI,Search]
---
# Traditional Methods
Traditional (optimization) methods:

* NOT robust.
* Effective when appropriate.
* two disjoint classes:
  * Only evaluate complete solutions.
    * e.g.
  * Require the evaluation of partially contructed and approximate solutions.
    * e.g.

Complete solution: all decision variables are specified.

e.g. 1111,0000,0101,1010

Partial solution:

* an incomplete solution to the original proble

  e.g. 11{00,01,10,11}, 01{00,01,10,11}
* a complete solution to a reduced problem.

  e.g. {00,01,10,11} | {00,01,10,11}

Additional difficulties posed by partial solutions:

* devise a way to organize the subspaces so that they can be searched efficiently. (Tree search)
* create a new evaluation function that can assess the quality of partial solutions. (Branch and bound)

Notice: Different representations of solutions -> different sizes of search space + potentially different manners of search (different optimization methods).

## Exhaustive Search
Recall that to solve a problem algorithmatically, we have to specify following three things:
1. the representation (of solution)
2. the objective
3. the evaluation function (ordinal or numeric)

> The only requirement for exhaustive search is to generate evey possible solution to the problem *systematically*.

We could reduce the amount of work in many ways, e.g. cut off some branches in a search tree.

### Enumerate SAT, TSP, NLP (Non-Linear Programming)
In SAT, we could exhaustively search solutions by DFS.

In TSP, we have to enumerate the permutations.

1. Mapping Between permutations and intergers
2. Recursive generation
3. Successive generation (e.g. in lexicographic ordering)

Notice: the specific algorithms could be checked on books and wiki.

In NLP, we use grid methods to approximate exhaustive search. The whole domain is divided into cells, the best of which could be futher divided for finer-granularity results.

## Local Search
Local Search:

1. Pick a solution as the *current* solution.
2. Apply a transformation in the given set to this solution to generate a new solution
3. If the *current*  one is better, discard the new solution; else replace the *current* one with the new solution.
4. Repeat 2. and 3. until no transformation improves the *current* solution.

The key lies in the type of trasformation (can be deterministic or stochastic). Two extremes are:
1. select a solution in search space uniformly at random. (Large neighbor)
2. only return the *current* solution. (Small neighbor)

Notice: hill climbing methods is similar to this procedure except the solution updating the current solution is required to be the best among all neighbors.

To avoid local optima, usually a large number of random initializations are required.

### SAT
#### GSAT
Random flip a binary string.

### TSP

#### K-opt
Change nonadjacent edges to generate new Hamiltonian cycles.

#### Lin-Kernighan

* Representation of solution is based on $\delta$-path. Hamiltonian cycle is implied. One $\delta$-path could generate two possible Hamiltonian cycles.
* Using a simple operation called *switch*, we could generate a new $delta$-path.

TODO: There are several lines in the book that are difficult to understand.

Anyway, this algorithm runs vary fast and generates near-optimal solutions (within 2 percent of optimum path).

### NLP
We also consider the problem of finding the zeros of a function. It could be treated as a optimization problem. For example, we evaluate how close the result is to 0.

#### Bracketing Methods
* Methods of bisecting the range.
* Faster convergence: Regula Flasi.

  [an image here]()
* Brent's method: relies on parabolic auxilary functions (Three points rather than two points in a bracket).


#### Fixed-Point Methods
* Newton's methods: Faster, but may even fail to convege for continous functions.

  * need good initial guess.
  * the calculation of derivatives may be computationally expensive.

  [an image here]()

* Secant methods: NOT Fixed-Point Methods! Newton's methods without derivatives.

#### Gradient Methods of Minimization

* make use of gradients.
* May incorporate second-order derivative (Hessian Matrix) information into update. (Newton's method)

## Linear Programming
### The Simplex Method
