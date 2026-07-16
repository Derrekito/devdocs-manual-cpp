# std::plus

```cpp
template< class T >
struct plus;  // (until C++14)
template< class T = void >
struct plus;  // (since C++14)
```

Function object for performing addition. Effectively calls `operator+` on two
instances of type `T`.

### Specializations

The standard library provides a specialization of `std::plus` when `T` is not
specified, which leaves the parameter types and return type to be deduced.
- **plus<void> (C++14)** — function object implementing `x + y` deducing
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

- ****operator()**** — returns the sum of two arguments (public member function)

## std::plus::operator()

```cpp
T operator()( const T& lhs, const T& rhs ) const;  // (until C++14)
constexpr T operator()( const T& lhs, const T& rhs ) const;  // (since C++14)
```

Returns the sum of `lhs` and `rhs`.

### Parameters

- **lhs, rhs** — values to sum

### Return value

The result of `lhs + rhs`.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr T operator()(const T& lhs, const T& rhs) const
{
    return lhs + rhs;
}
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/plus*
