# C++ named requirements: TrivialType (since C++11)

Specifies that a type is a trivial type.

Note: the standard doesn't define a named requirement with this name. This is a
type category defined by the core language. It is included here as a named
requirement only for consistency.

### Requirements

The following types are collectively called *trivial types*:

- scalar types
- trivial class types
- arrays of such types
- cv-qualified versions of these types

### See also

- **is_trivial (C++11)** — checks if a type is trivial (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/TrivialType*
