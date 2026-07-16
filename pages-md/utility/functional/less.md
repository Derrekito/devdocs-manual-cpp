# std::less

```cpp
template< class T >
struct less;  // (until C++14)
template< class T = void >
struct less;  // (since C++14)
```

Function object for performing comparisons. The main template invokes
`operator<` on type `T`.

### Specializations

- **less<void> (C++14)** — function object implementing `x < y` deducing
  argument and return types (class template specialization)

### Member types

- **`result_type` (deprecated in C++17)(removed in C++20)** — bool
- **`first_argument_type` (deprecated in C++17)(removed in C++20)** — `T`
- **`second_argument_type` (deprecated in C++17)(removed in C++20)** — `T`

These member types are obtained via publicly inheriting std::binary_function<T,
T, bool>.
*(until C++11)*

### Member functions

- **operator()** — checks whether the first argument is *less* than the second
  (public member function)

## std::less::operator()

```cpp
bool operator()( const T& lhs, const T& rhs ) const;  // (until C++14)
constexpr bool operator()( const T& lhs, const T& rhs ) const;  // (since C++14)
```

Checks whether `lhs` is *less* than `rhs`.

### Parameters

- **lhs, rhs** — values to compare

### Return value

`lhs < rhs`.

If `T` is a pointer type, the result is consistent with the
implementation-defined strict total order over pointers.

### Exceptions

May throw implementation-defined exceptions.

### Possible implementation

```cpp
constexpr bool operator()(const T& lhs, const T& rhs) const
{
    return lhs < rhs; // assumes that the implementation handles pointer total order
}
```

### Example

```cpp
#include <functional>

template<typename A, typename B, typename C = std::less<>>
constexpr bool fun(A a, B b, C cmp = C{})
{
    return cmp(a, b);
}

static_assert(fun(1, 2) == true);
static_assert(fun(1.0, 1) == false);
static_assert(fun(1, 2.0) == true);
static_assert(std::less<int>{}(5, 5.6) == false);   // 5 < 5 (warn: implicit conversion)
static_assert(std::less<double>{}(5, 5.6) == true); // 5.0 < 5.6
static_assert(std::less<int>{}(5.6, 5.7) == false); // 5 < 5 (warn: implicit conversion)
static_assert(std::less{}(5, 5.6) == true);         // less<void>: 5.0 < 5.6

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2562 | C++98 | the pointer total order might be inconsistent | guaranteed
      to be consistent

### See also

- **equal_to** — function object implementing `x == y` (class template)
- **greater** — function object implementing `x > y` (class template)
- **ranges::less (C++20)** — function object implementing `x < y` (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/less*
