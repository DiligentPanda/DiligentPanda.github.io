---
title: Cython for NumPy
date: 2017-07-16 14:01:28
categories: Programming/Python
tags: [cython, python, numpy]
---

# Cython for NumPy

## Cython

* A compiler
> Cython is an optimizing static compiler for both the Python and the extended Cython.

* A language
> Cython is a language that makes writing C extensions for the Python language as easy as Python itself.

### Speed-Up
In the later discussion, we will achieve speed-up from the following aspects.
* static data type
* no negative indices
* no boundary check
* eliminates overhead of for-loop
* eliminates overhead of fuction calls: specify type of parameters and return values
* efficient indexing
* ...

Anyway, the main ideas here are:

* Whatever is convenient in Python may be slow in execution. We convert those part into explicit coding in C to speed up the process. (Even though always a C file is generated for .pyx file, but runing of some codes need python interpreter, which makes execution slow.)
* Those functionalities in Python are usually more generic. So we have to provide addtional information. If the compiler lacks of some information, the codes still runs in Python way rather than C. (For newbies like me, we don't know how much information is sufficient for cython's optimization.)
* In another direction, we want to make use of many useful tools already implementd in Pyhon.For instance, We want to exploit Numpy, which provides convenient API of Multi-dimensional arrays. However, note that most of time-comsuming operations of this library are implemented in C fundamentally, which means when we call those functions, we would gain little speed-up.

### Steps of Using Cython
1. write .pyx source file
2. run Cython compiler: generate C file
3. run C compiler: generate a compiled library(.so, a shared library)
4. import the libaray into other modules.

It is very __IMPORTANT__ to check whether we obtain expected elegant C codes in .c file.

### Compile Libaray by *setup.py*

```python
from distutils.core import setup
from Cython.Build import cythonize

setup(
  ext_module=cythonize("xxx.pyx")
)
```

### Syntax

1. First of all, we could write any python codes. When it is possible to be converted to C implementation, it would speed up.

2. `cdef` to add types for variables:

```python
cdef int i = 1
cdef np.ndarray x = np.zeros(...)
```
For efficient indexing, we also specify the data type and the number of dimensions for `ndarray`.

```python
cdef np.ndarray[DTYPE_t, ndim=2] x = np.zeros(...)
```

Note that it can only be used at the top indentation level.

3. `cdef` can also be used to define functions: it is used for Cython functions that are intended to be pure 'C' functions. Types of parameters and return value should be specified. However, this function can not be called in Python. We are writing Python codes, but evetually it is C code.

Functions defined by `def` can be called direcly from Python codes. We can specify types for parameters of the function. (Return value as local variable can also be specified with a type.) However, it incurs a high overhead.

Besides, there is `cpdef`, which is both.

We could futher use `inline` for `cdef` function.

```python
cdef inline int max(int a,int b): return a if a>=b else b
```

4. Notice there is two type of *TYPE* here: one is used for compilation, another is for run-time. In those python codes we write, run-time types are used, which are exactly the same as what we used in python, e.g. `np.int`. Types like `int` (and other default types in C like `unsigned int` ...) or `np.int_t` (In NumPy, suffix `_t` means the type is used for compilation.? I guess this is only for those default type.) are used to specify types for compilation.

`ctypedef` have the same usage as `#typedef` in C. `cimport` is used to import special compile-time information, such as `np.int_t`. It acts like `#include`.

```python
import numpy as np
cimport numpy as np
DTYPE=np.int # run-time  type
ctypedef np.int_t DTYPE_t# compile-time type
```

5. Specify iteration variable also help with fast loops.
```python
cdef int i
for i in range(100):
  ...
```

We have to check whether pure C-loop is generated in the .c file.

6. Even faster indexing:
  * using `@cython.boundscheck(False)` could turn off boundary check.
  * Force index variable to be `unsigned int` to eliminate negative indexing.
  ```python
  i = <unsigned int> i # casting if needed
  ```
