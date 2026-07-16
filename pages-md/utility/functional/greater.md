# std::greater

```cpp
template< class T >
struct greater;  // (until C++14)
template< class T = void >
struct greater;  // (since C++14)
```

Function object for performing comparisons. The main template invokes
`operator>` on type `T`.

### Specializations

- **greater<void> (C++14)** — function object implementing `x > y` deducing
  argument and return types (class template specialization)

### Member types

- **`result_type` (deprecated in C++17)(removed in C++20)** — bool
- **`first_argument_type` (deprecated in C++17)(removed in C++20)** — `T`
- **`second_argument_type` (deprecated in C++17)(removed in C++20)** — `T`

These member types are obtained via publicly inheriting std::binary_function<T,
T, bool>.
*(until C++11)*

### Member functions

- **operator()** — checks whether the first argument is *greater* than the
  second (public member function)

## std::greater::operator()

```cpp
bool operator()( const T& lhs, const T& rhs ) const;  // (until C++14)
constexpr bool operator()( const T& lhs, const T& rhs ) const;  // (since C++14)
```

Checks whether `lhs` is *greater* than `rhs`.

### Parameters

- **lhs, rhs** — values to compare

### Return value

`lhs > rhs`.

If `T` is a pointer type, the result is consistent with the
implementation-defined strict total order over pointers.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr bool operator()(const T& lhs, const T& rhs) const
{
    return lhs > rhs; // assumes that the implementation handles pointer total order
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2562 | C++98 | the pointer total order might be inconsistent | guaranteed
      to be consistent

### See also

- **less** — function object implementing `x < y` (class template)
- **ranges::greater (C++20)** — function object implementing `x > y` (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/greater*
