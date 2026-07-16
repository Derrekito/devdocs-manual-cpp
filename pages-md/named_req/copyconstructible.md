# C++ named requirements: CopyConstructible

Specifies that an instance of the type can be copy-constructed from an lvalue
expression.

### Requirements

The type `T` satisfies CopyConstructible if

- The type `T` satisfies MoveConstructible, and

Given

- `v`, an lvalue expression of type `T` or const T or an rvalue expression of
  type const T,
- `u`, an arbitrary identifier.

The following expressions must be valid and have their specified effects:

  Expression | Post-conditions
  `T u = v;` | The value of `u` is equivalent to the value of `v`. The value of
      `v` is unchanged.
  `T(v)` | The value of `T(v)` is equivalent to the value of `v`. The value of
      `v` is unchanged.

The expression `v.~T()` also must be valid, and, for lvalue `v`, the expression
`&v` must have the type `T*` or const T* and must evaluate to the address of
`v`.
*(until C++11)*

### Notes

Until C++11, classes that overloaded operator& were not CopyConstructible and
thus were not usable in the standard library containers. This is a design
decision in C++98 (instead of a defect, see LWG issue 390).

Since C++11, the standard library uses `std::addressof` whenever the address of
an object is needed.

### See also

-
  **is_copy_constructibleis_trivially_copy_constructibleis_nothrow_copy_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a copy constructor (class
  template)
- **copy_constructible (C++20)** — specifies that an object of a type can be
  copy constructed and move constructed (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/CopyConstructible*
