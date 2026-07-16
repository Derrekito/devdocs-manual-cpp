# std::unary_negate

```cpp
template< class Predicate >
struct unary_negate : public std::unary_function<Predicate::argument_type, bool>;  // (until C++11)
template< class Predicate >
struct unary_negate;  // (since C++11) (deprecated in C++17) (removed in C++20)
```

`std::unary_negate` is a wrapper function object returning the complement of the
unary predicate it holds.

The unary predicate type must define a member type, `argument_type`, that is
convertible to the predicate's parameter type. The unary function objects
obtained from `std::ref`, `std::cref`, `std::negate`, `std::logical_not`,
`std::mem_fn`, `std::function`, `std::hash`, or from another call to `std::not1`
have this type defined, as are function objects derived from the deprecated
`std::unary_function`.

`std::unary_negate` objects are easily constructed with helper function
`std::not1`.

### Member types

- **`argument_type`** — `Predicate::argument_type`
- **`result_type`** — `bool`

### Member functions

- **(constructor)** — constructs a new unary_negate object with the supplied
  predicate (public member function)
- **operator()** — returns the logical complement of the result of a call to the
  stored predicate (public member function)

## std::unary_negate::unary_negate

```cpp
explicit unary_negate( Predicate const& pred );  // (until C++14)
constexpr explicit unary_negate( Predicate const& pred );  // (since C++14)
```

Constructs a `std::unary_negate` function object with the stored predicate
`pred`.

### Parameters

- **pred** — predicate function object

## std::unary_negate::operator()

```cpp
bool operator()( argument_type const& x ) const;  // (until C++14)
constexpr bool operator()( argument_type const& x ) const;  // (since C++14)
```

Returns the logical complement of the result of calling `pred(x)`.

### Parameters

- **x** — argument to pass through to predicate

### Return value

The logical complement of the result of calling `pred(x)`.

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

struct less_than_7 : std::unary_function<int, bool>
{
    bool operator()(int i) const { return i < 7; }
};

int main()
{
    std::vector<int> v(7, 7);
    v[0] = v[1] = v[2] = 6;

    std::unary_negate<less_than_7> not_less_than_7((less_than_7()));
    // C++11 solution:
    // Use std::function<bool (int)>
    // std::function<bool (int)> not_less_than_7 =
    //     [](int x)->bool { return !less_than_7()(x); };

    std::cout << std::count_if(v.begin(), v.end(), not_less_than_7);
}
```

Output:

```text
4
```

### See also

- **binary_negate (deprecated in C++17)(removed in C++20)** — wrapper function
  object returning the complement of the binary predicate it holds (class
  template)
- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **not1 (deprecated in C++17)(removed in C++20)** — constructs custom
  `std::unary_negate` object (function template)
- **ptr_fun (deprecated in C++11)(removed in C++17)** — creates an
  adaptor-compatible function object wrapper from a pointer to function
  (function template)
- **unary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible unary function base class (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/unary_negate*
