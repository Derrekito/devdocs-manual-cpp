# std::not_equal_to

```cpp
template< class T >
struct not_equal_to;  // (until C++14)
template< class T = void >
struct not_equal_to;  // (since C++14)
```

Function object for performing comparisons. Unless specialised, invokes
`operator!=` on type `T`.

### Specializations

The standard library provides a specialization of `std::not_equal_to` when `T`
is not specified, which leaves the parameter types and return type to be
deduced.
- **not_equal_to<void> (C++14)** — function object implementing `x != y`
  deducing argument and return types (class template specialization)
*(since C++14)*

### Member types

- **`result_type` (deprecated in C++17)(removed in C++20)** — bool
- **`first_argument_type` (deprecated in C++17)(removed in C++20)** — `T`
- **`second_argument_type` (deprecated in C++17)(removed in C++20)** — `T`

These member types are obtained via publicly inheriting std::binary_function<T,
T, bool>.
*(until C++11)*

### Member functions

- **operator()** — checks if the arguments are *not equal* (public member
  function)

## std::not_equal_to::operator()

```cpp
bool operator()( const T& lhs, const T& rhs ) const;  // (until C++14)
constexpr bool operator()( const T& lhs, const T& rhs ) const;  // (since C++14)
```

Checks whether `lhs` is *not equal* to `rhs`.

### Parameters

- **lhs, rhs** — values to compare

### Return value

`true` if `lhs != rhs`, `false` otherwise.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr bool operator()(const T& lhs, const T& rhs) const
{
    return lhs != rhs;
}
```

### See also

- **equal** — determines if two sets of elements are the same (function
  template)
- **less** — function object implementing `x < y` (class template)
- **ranges::not_equal_to (C++20)** — function object implementing `x != y`
  (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/not_equal_to*
