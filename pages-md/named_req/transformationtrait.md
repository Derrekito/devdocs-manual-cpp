# C++ named requirements: TransformationTrait (since C++11)

A **TransformationTrait** is a class template that provides a transformation of
its template type parameter.

### Requirements

- Takes one template type parameter (additional template parameters are optional
  and allowed).
- The transformed type is a publicly accessible nested type named `type`.

### Standard library

All type transformation traits satisfy **TransformationTrait**.

The following standard library class templates also satisfy
**TransformationTrait**.

- **std::tuple_element<std::tuple> (C++11)** — obtains the type of the specified
  element (class template specialization)
- **std::tuple_element<std::pair> (C++11)** — obtains the type of the elements
  of `pair` (class template specialization)
- **std::tuple_element<std::array> (C++11)** — obtains the type of the elements
  of `array` (class template specialization)
- **variant_alternativevariant_alternative_t (C++17)** — obtains the type of the
  alternative specified by its index, at compile time (class template) (alias
  template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/TransformationTrait*
