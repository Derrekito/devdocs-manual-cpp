# C++ named requirements: Hash (since C++11)

A **Hash** is a function object for which the output depends only on the input
and has a very low probability of yielding the same output given different input
values.

### Requirements

The type `T` satisfies Hash if

- The type `T` satisfies FunctionObject, CopyConstructible, Destructible, and

Given

- `h`, a value of type `T` or `const T`, whose argument type is `Key`,
- `k`, a value of type convertible to `Key` or `const Key`,
- `u`, an lvalue expression of type `Key`.

The following expressions must be valid and have their specified effects.

  Expression | Return type | Requirements
  `h(k)` | `std::size_t` | The returned value depends only on the value of `k`
      for the duration of the program. All evaluations of `h(k)` executed within
      a given execution of a program yield the same result for the same value of
      `k`. The probability of `h(a) == h(b)` for `a != b` should approach `1.0 /
      std::numeric_limits<std::size_t>::max()`.
  `h(u)` | `std::size_t` | `u` is not modified.

### Standard library

- **hash (C++11)** — hash function object (class template)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2291 | C++11 | same results for same arguments were required in all cases
      | only required within a single execution

---
*Source: https://en.cppreference.com/w/cpp/named_req/Hash*
