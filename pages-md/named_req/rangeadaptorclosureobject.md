# C++ named requirements: RangeAdaptorClosureObject (since C++20)

*Range adaptor closure objects* are FunctionObjects that are callable via the
pipe operator: if `C` is a range adaptor closure object and `R` is a `range`,
these two expressions are equivalent:

```cpp
C(R)
R | C
```

Two range adaptor closure objects can be chained by `operator|` to produce
another range adaptor closure object: if `C` and `D` are range adaptor closure
objects, then `C | D` produces a range adaptor closure object `E` with the
following properties:

- `E` stores a copy of `C` and `D`, direct-non-list-initialized from
  `std::forward<decltype((C))>(C)` and `std::forward<decltype((D))>(D)`
  respectively. If such initialization is invalid, `C | D` is also invalid.
- Let `c` and `d` be the stored copies (with the same constness and value
  category as `E`), and `R` be a `range` object, the following expressions are
  equivalent:

```cpp
d(c(R))
R | c | d
E(R)
R | E // R | (C | D)
```

Notes: `operator()` is unsupported for volatile-qualified or
const-volatile-qualified version of range adaptor object closure types.

Objects whose type is the same as one of the following objects (ignoring
cv-qualification) are range adaptor closure objects:

- unary range adaptor objects,

- objects of user-defined types that meet the requirements of implementing a
  range adaptor closure object,
*(since C++23)*

- the results of binding trailing arguments by range adaptor objects, and
- the results of chaining two range adaptor closure objects by `operator|`.

---
*Source: https://en.cppreference.com/w/cpp/named_req/RangeAdaptorClosureObject*
