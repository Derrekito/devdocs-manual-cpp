# C++ named requirements: MoveConstructible (since C++11)

Specifies that an instance of the type can be constructed from an rvalue
argument.

### Requirements

The type `T` satisfies MoveConstructible if

Given

- `rv`, an rvalue expression of type `T`,
- `u`, an arbitrary identifier.

The following expressions must be valid and have their specified effects.

  Expression | Post-conditions
  `T u = rv;` | The value of `u` is equivalent to the value of `rv` before the
      initialization. The new value of `rv` is unspecified.
  `T(rv)` | The value of `T(rv)` is equivalent to the value of `rv` before the
      initialization. The new value of `rv` is unspecified.

### Notes

A class does not have to implement a move constructor to satisfy this type
requirement: a copy constructor that takes a `const T&` argument can bind rvalue
expressions.

If a MoveConstructible class implements a move constructor, it may also
implement move semantics to take advantage of the fact that the value of `rv`
after construction is unspecified.

### See also

-
  **is_move_constructibleis_trivially_move_constructibleis_nothrow_move_constructible
  (C++11)(C++11)(C++11)** — checks if a type can be constructed from an rvalue
  reference (class template)
- **move_constructible (C++20)** — specifies that an object of a type can be
  move constructed (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/MoveConstructible*
