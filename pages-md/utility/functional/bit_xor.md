# std::bit_xor

```cpp
template< class T >
struct bit_xor;  // (until C++14)
template< class T = void >
struct bit_xor;  // (since C++14)
```

Function object for performing bitwise XOR. Effectively calls `operator^` on
type `T`.

### Specializations

The standard library provides a specialization of `std::bit_xor` when `T` is not
specified, which leaves the parameter types and return type to be deduced.
- **bit_xor<void> (C++14)** — function object implementing `x ^ y` deducing
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

- **operator()** — returns the result of bitwise XOR of two arguments (public
  member function)

## std::bit_xor::operator()

```cpp
T operator()( const T& lhs, const T& rhs ) const;  // (until C++14)
constexpr T operator()( const T& lhs, const T& rhs ) const;  // (since C++14)
```

Returns the result of bitwise XOR of `lhs` and `rhs`.

### Parameters

- **lhs, rhs** — values to compute bitwise XOR of

### Return value

The result of `lhs ^ rhs`.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr T operator()(const T& lhs, const T& rhs) const
{
    return lhs ^ rhs;
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 660 | C++98 | function objects for bitwise operations are missing | added

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/bit_xor*
