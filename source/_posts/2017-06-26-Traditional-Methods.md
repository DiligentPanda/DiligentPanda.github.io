---
title: Traditional Methods
date: 2017-06-26 13:14:28
categories:
tags:
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

Different representations of solutions = different sizes of search space + potentially different manners of search (different optimization methods).

## Exhaustive Search
