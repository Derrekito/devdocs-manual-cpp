# std::modulus

```cpp
template< class T >
struct modulus;  // (until C++14)
template< class T = void >
struct modulus;  // (since C++14)
```

Function object for computing remainders of divisions. Implements `operator%`
for type `T`.

### Specializations

The standard library provides a specialization of `std::modulus` when `T` is not
specified, which leaves the parameter types and return type to be deduced.
- **modulus<void> (C++14)** — function object implementing `x % y` deducing
  argument and return types (class template specialization)
*(since C++14)*

### Member types

- **`result_type` (deprecated in C++17)(removed in C++20)** — `T`
- **`first_argument_type` (deprecated in C++17)(removed in C++20)** — `T`
- **`second_argument_type` (deprecated in C++17)(removed in C++20)** — `T`

These member types are obtained via publicly inheriting std::binary_function<T,
T, T>.
*(until C++11)*

### Member functions

- **operator()** — returns the remainder from the division of the first argument
  by the second argument (public member function)

## std::modulus::operator()

```cpp
T operator()( const T& lhs, const T& rhs ) const;  // (until C++14)
constexpr T operator()( const T& lhs, const T& rhs ) const;  // (since C++14)
```

Returns the remainder of the division of `lhs` by `rhs`.

### Parameters

- **lhs, rhs** — values to divide one by another

### Return value

The result of `lhs % rhs`.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr T operator()(const T& lhs, const T& rhs) const
{
    return lhs % rhs;
}
```

### See also

- **fmodfmodffmodl (C++11)(C++11)** — remainder of the floating point division
  operation (function)
- **remainderremainderfremainderl (C++11)(C++11)(C++11)** — signed remainder of
  the division operation (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/modulus*
