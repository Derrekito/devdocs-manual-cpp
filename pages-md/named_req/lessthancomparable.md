# C++ named requirements: LessThanComparable

The type must work with `<` operator and the result should have standard
semantics.

### Requirements

The type `T` satisfies LessThanComparable if

Given

- `a`, `b`, and `c`, expressions of type `T` or `const T`.

The following expressions must be valid and have their specified effects.

  Expression | Return type | Requirements
  `a < b` | implicitly convertible to `bool` | Establishes strict weak ordering
      relation with the following properties: For all `a`, `!(a < a)`. If `a <
      b` then `!(b < a)`. If `a < b` and `b < c` then `a < c`. Defining
      `equiv(a, b)` as `!(a < b) && !(b < a)`, if `equiv(a, b)` and `equiv(b,
      c)`, then `equiv(a, c)`.

### Notes

To satisfy this requirement, types that do not have built-in comparison
operators have to provide a user-defined `operator<`.

For the types that are both EqualityComparable and LessThanComparable, the C++
standard library makes a distinction between

- *Equality*, which is the value of the expression `a == b` and
- *Equivalence*, which is the value of the expression `!(a < b) && !(b < a)`.

### See also

- **Compare** — a BinaryPredicate that establishes an ordering relation (named
  requirement)
- **strict_weak_order (C++20)** — specifies that a `relation` imposes a strict
  weak ordering (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/LessThanComparable*
