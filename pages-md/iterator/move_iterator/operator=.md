# std::move_iterator<Iter>::operator=

```cpp
template< class U >
move_iterator& operator=( const move_iterator<U>& other );  // (until C++17)
template< class U >
constexpr move_iterator& operator=( const move_iterator<U>& other );  // (since C++17)
```

The underlying iterator is assigned the value of the underlying iterator of
`other`, i.e. `other.base()`.

The behavior is undefined if `U` is not convertible to `Iter`.
*(until C++20)*

This overload participates in overload resolution only if `U` is not the same
type as `Iter` and `std::convertible_to<const U&, Iter>` and
`std::assignable_from<Iter&, const U&>` are modeled.
*(since C++20)*

### Parameters

- **other** — iterator adaptor to assign

### Return value

`*this`

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3435 | C++20 | the converting assignment operator was not constrained |
      constrained

### See also

- **(constructor) (C++11)** — constructs a new iterator adaptor (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/operator%3D*
