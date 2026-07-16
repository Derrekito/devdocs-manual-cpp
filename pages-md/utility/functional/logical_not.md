# std::logical_not

```cpp
template< class T >
struct logical_not;  // (until C++14)
template< class T = void >
struct logical_not;  // (since C++14)
```

Function object for performing logical NOT (logical negation). Effectively calls
`operator!` for type `T`.

### Specializations

The standard library provides a specialization of `std::logical_not` when `T` is
not specified, which leaves the parameter types and return type to be deduced.
- **logical_not<void> (C++14)** — function object implementing `!x` deducing
  argument and return types (class template specialization)
*(since C++14)*

### Member types

- **`result_type` (deprecated in C++17)(removed in C++20)** — bool
- **`argument_type` (deprecated in C++17)(removed in C++20)** — `T`

These member types are obtained via publicly inheriting std::unary_function<T,
bool>.
*(until C++11)*

### Member functions

- **operator()** — returns the logical NOT of the argument (public member
  function)

## std::logical_not::operator()

```cpp
bool operator()( const T& arg ) const;  // (until C++14)
constexpr bool operator()( const T& arg ) const;  // (since C++14)
```

Returns the logical NOT of `arg`.

### Parameters

- **arg** — value to compute logical NOT of

### Return value

The result of `!arg`.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr // since C++14
bool operator()(const T& arg) const
{
    return !arg;
}
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/logical_not*
