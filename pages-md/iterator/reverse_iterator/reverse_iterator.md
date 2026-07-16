# std::reverse_iterator<Iter>::reverse_iterator

```cpp
reverse_iterator();  // (until C++17)
constexpr reverse_iterator();  // (since C++17)
explicit reverse_iterator( iterator_type x );  // (until C++17)
constexpr explicit reverse_iterator( iterator_type x );  // (since C++17)
template< class U >
reverse_iterator( const reverse_iterator<U>& other );  // (until C++17)
template< class U >
constexpr reverse_iterator( const reverse_iterator<U>& other );  // (since C++17)
```

Constructs a new iterator adaptor.

1) Default constructor. The underlying iterator is value-initialized. Operations
   on the resulting iterator have defined behavior if and only if the
   corresponding operations on a value-initialized `Iter` also have defined
   behavior.

2) The underlying iterator is initialized with `x`.

3) The underlying iterator is initialized with that of `other`. This overload
   participates in overload resolution only if `U` is not the same type as
   `Iter` and `std::convertible_to<const U&, Iter>` is modeled(since C++20).

### Parameters

- **x** — iterator to adapt
- **other** — iterator adaptor to copy

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 235 | C++98 | the effect of the default constructor was not specified |
      specified
  LWG 1012 | C++98 | the underlying iterator was default-initialized | it is
      value-initialized
  LWG 3435 | C++20 | the converting constructor from another `reverse_iterator`
      was not constrained | constrained

### See also

- **operator=** — assigns another iterator adaptor (public member function)
- **make_reverse_iterator (C++14)** — creates a `std::reverse_iterator` of type
  inferred from the argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/reverse_iterator/reverse_iterator*
