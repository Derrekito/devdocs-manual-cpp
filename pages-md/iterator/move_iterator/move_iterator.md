# std::move_iterator<Iter>::move_iterator

```cpp
move_iterator();  // (until C++17)
constexpr move_iterator();  // (since C++17)
explicit move_iterator( iterator_type x );  // (until C++17)
constexpr explicit move_iterator( iterator_type x );  // (since C++17)
template< class U >
move_iterator( const move_iterator<U>& other );  // (until C++17)
template< class U >
constexpr move_iterator( const move_iterator<U>& other );  // (since C++17)
```

Constructs a new iterator adaptor.

1) Default constructor. The underlying iterator is value-initialized. Operations
   on the resulting iterator have defined behavior if and only if the
   corresponding operations on a value-initialized `Iter` also have defined
   behavior.

2) The underlying iterator is initialized with `x`(until
   C++20)`std::move(x)`(since C++20).

3) The underlying iterator is initialized with that of `other`. The behavior is
   undefined if `U` is not convertible to `Iter`(until C++20)This overload
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
  LWG 3435 | C++20 | the converting constructor from another `move_iterator` was
      not constrained | constrained

### See also

- **operator= (C++11)** — assigns another iterator adaptor (public member
  function)
- **make_move_iterator (C++11)** — creates a `std::move_iterator` of type
  inferred from the argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/move_iterator*
