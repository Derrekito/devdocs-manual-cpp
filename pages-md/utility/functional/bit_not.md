# std::bit_not

```cpp
template< class T = void >
struct bit_not;  // (since C++14)
```

Function object for performing bitwise NOT. Effectively calls `operator~` on
type `T`.

### Specializations

The standard library provides a specialization of `std::bit_not` when `T` is not
specified, which leaves the parameter types and return type to be deduced.

- **bit_not<void> (C++14)** — function object implementing `~x` deducing
  argument and return types (class template specialization)

### Member types

- **`result_type` (deprecated in C++17)(removed in C++20)** — `T`
- **`argument_type` (deprecated in C++17)(removed in C++20)** — `T`

### Member functions

- **operator() (C++14)** — returns the result of bitwise NOT of its argument
  (public member function)

## std::bit_not::operator()

```cpp
constexpr T operator()( const T& arg ) const;  // (since C++14)
```

Returns the result of bitwise NOT of `arg`.

### Parameters

- **arg** — value to compute bitwise NOT of

### Return value

The result of `~arg`.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr T operator()(const T& arg) const
{
    return ~arg;
}
```

### Notes

Although `std::bit_not` is added via post-C++11 proposal N3421, it is treated as
a part of the resolution for LWG issue 660 (except for its transparent
specialization `std::bit_not<>`) by common implementations, and thus available
in their C++98/03 mode.

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/bit_not*
