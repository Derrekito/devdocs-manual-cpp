# std::negate

```cpp
template< class T >
struct negate;  // (until C++14)
template< class T = void >
struct negate;  // (since C++14)
```

Function object for performing negation. Effectively calls `operator-` on an
instance of type `T`.

### Specializations

The standard library provides a specialization of `std::negate` when `T` is not
specified, which leaves the parameter types and return type to be deduced.
- **negate<void> (C++14)** — function object implementing `-x` deducing argument
  and return types (class template specialization)
*(since C++14)*

### Member types

- **`result_type` (deprecated in C++17)(removed in C++20)** — `T`
- **`argument_type` (deprecated in C++17)(removed in C++20)** — `T`

These member types are obtained via publicly inheriting std::unary_function<T,
T>.
*(until C++11)*

### Member functions

- **operator()** — returns the negation of the argument (public member function)

## std::negate::operator()

```cpp
T operator()( const T& arg ) const;  // (until C++14)
constexpr T operator()( const T& arg ) const;  // (since C++14)
```

Returns the negation of `arg`.

### Parameters

- **arg** — value to compute negation of

### Return value

The result of `-arg`.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr T operator()(const T& arg) const
{
    return -arg;
}
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/negate*
