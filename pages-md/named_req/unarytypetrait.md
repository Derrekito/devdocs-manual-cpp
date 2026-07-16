# C++ named requirements: UnaryTypeTrait (since C++11)

A **UnaryTypeTrait** is a class template that describes a property of its
template type parameter with an `std::integral_constant` (typically
`std::bool_constant`) specialization.

### Requirements

- DefaultConstructible and CopyConstructible.
- Takes one template type parameter (additional template parameters are optional
  and allowed).
- Publicly and unambiguously derived from a specialization of
  `std::integral_constant`, known as its *base characteristic*.
- The member names of the *base characteristic* are not hidden and are
  unambiguously available.

### Standard library

The following type traits all satisfy UnaryTypeTrait:

- Primary type categories
- Composite type categories
- Type properties
- Supported operations
- Property queries

The following standard library class templates also satisfy UnaryTypeTrait.

- **integral_constant (C++11)** — compile-time constant of specified type with
  specified value (class template)
- **negation (C++17)** — logical NOT metafunction (class template)
- **std::tuple_size<std::tuple> (C++11)** — obtains the size of `tuple` at
  compile time (class template specialization)
- **std::tuple_size<std::array> (C++11)** — obtains the size of an `array`
  (class template specialization)
- **std::tuple_size<std::pair> (C++11)** — obtains the size of a `pair` (class
  template specialization)
- **variant_sizevariant_size_v (C++17)** — obtains the size of the variant's
  list of alternatives at compile time (class template) (variable template)
- **is_bind_expression (C++11)** — indicates that an object is `std::bind`
  expression or can be used as one (class template)
- **is_placeholder (C++11)** — indicates that an object is a standard
  placeholder or can be used as one (class template)
- **is_execution_policy (C++17)** — test whether a class represents an execution
  policy (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/UnaryTypeTrait*
