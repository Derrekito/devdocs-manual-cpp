# C++ named requirements: BinaryTypeTrait (since C++11)

A **BinaryTypeTrait** is a class template that describes a relationship between
its two template type parameters with a `std::integral_constant` (typically
`std::bool_constant`) specialization.

### Requirements

- DefaultConstructible and CopyConstructible.
- Takes two template type parameters (additional template parameters are
  optional and allowed).
- Publicly and unambiguously derived from a specialization of
  `std::integral_constant`, known as its *base characteristic*.
- The member names of the *base characteristic* are not hidden and are
  unambiguously available.

### Standard library

The following standard library class templates satisfy **BinaryTypeTrait**:

- **is_same (C++11)** — checks if two types are the same (class template)
- **is_base_of (C++11)** — checks if a type is derived from the other type
  (class template)
- **is_convertibleis_nothrow_convertible (C++11)(C++20)** — checks if a type can
  be converted to the other type (class template)
- **is_layout_compatible (C++20)** — checks if two types are *layout-compatible*
  (class template)
- **is_pointer_interconvertible_base_of (C++20)** — checks if a type is a
  *pointer-interconvertible* (initial) base of another type (class template)
- **is_invocableis_invocable_ris_nothrow_invocableis_nothrow_invocable_r
  (C++17)** — checks if a type can be invoked (as if by `std::invoke`) with the
  given argument types (class template)
- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/BinaryTypeTrait*
