# C++ named requirements: PODType (deprecated in C++20)

Specifies that the type is POD (Plain Old Data) type. This means the type is
compatible with the types used in the C programming language, that is, can be
exchanged with C libraries directly, in its binary form.

Note: the standard doesn't define a named requirement with this name. This is a
type category defined by the core language. It is included here as a named
requirement only for consistency.

This type requirement is deprecated in the C++ standard. All of its uses have
been replaced by the more refined type requirements, such as TrivialType,
ScalarType, or StandardLayoutType.
*(since C++20)*

### Requirements

The following types are collectively called *POD types*:

- scalar types
- POD classes
- arrays of such types
- cv-qualified versions of these types

### See also

- **is_pod (C++11)(deprecated in C++20)** — checks if a type is a plain-old data
  (POD) type (class template)
- **is_scalar (C++11)** — checks if a type is a scalar type (class template)
- **is_trivial (C++11)** — checks if a type is trivial (class template)
- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/PODType*
