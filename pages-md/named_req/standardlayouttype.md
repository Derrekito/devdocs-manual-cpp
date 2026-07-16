# C++ named requirements: StandardLayoutType (since C++11)

Specifies that a type is a standard layout type. Standard layout types are
useful for communicating with code written in other programming languages.

Note: the standard doesn't define a named requirement with this name. This is a
type category defined by the core language. It is included here as a named
requirement only for consistency.

### Requirements

The following types are collectively called *standard-layout types*:

- scalar types
- standard-layout class types
- arrays of such types
- cv-qualified versions of these types

### Properties

See Standard-layout.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1672 | C++11 | first non-static data member rule ignored existence of
      empty base classes | first non-static data member rule made recursive
  CWG 1813 | C++11 | class with a member defined in an indirect base wasn't
      technically standard-layout | all member declarations must be in the same
      class
  CWG 2120 | C++11 | array as first member wasn't considered when comparing the
      first element type with type of a base | array members are considered

### See also

- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/StandardLayoutType*
