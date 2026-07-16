# std::binary_negate

```cpp
template< class Predicate >
struct binary_negate
    : public std::binary_function<
        Predicate::first_argument_type,
        Predicate::second_argument_type,
        bool
    >;  // (until C++11)
template< class Predicate >
struct binary_negate;  // (since C++11) (deprecated in C++17) (removed in C++20)
```

`std::binary_negate` is a wrapper function object returning the complement of
the binary predicate it holds.

The binary predicate type must define two member types, `first_argument_type`
and `second_argument_type`, that are convertible to the predicate's parameter
types. The function objects obtained from `std::owner_less`, `std::ref`,
`std::cref`, `std::plus`, `std::minus`, `std::multiplies`, `std::divides`,
`std::modulus`, `std::equal_to`, `std::not_equal_to`, `std::greater`,
`std::less`, `std::greater_equal`, `std::less_equal`, `std::logical_not`,
`std::logical_or`, `std::bit_and`, `std::bit_or`, `std::bit_xor`, `std::mem_fn`,
`std::map::value_comp`, `std::multimap::value_comp`, `std::function`, or from a
call to `std::not2` have these types defined, as are function objects derived
from the deprecated `std::binary_function`.

`std::binary_negate` objects are easily constructed with helper function
`std::not2`.

### Member types

- **`first_argument_type`** — `Predicate::first_argument_type`
- **`second_argument_type`** — `Predicate::second_argument_type`
- **`result_type`** — `bool`

### Member functions

- **(constructor)** — constructs a new binary_negate object with the supplied
  predicate (public member function)
- **operator()** — returns the logical complement of the result of a call to the
  stored predicate (public member function)

## std::binary_negate::binary_negate

```cpp
explicit binary_negate( Predicate const& pred );  // (until C++14)
constexpr explicit binary_negate( Predicate const& pred );  // (since C++14)
```

Constructs a `std::binary_negate` function object with the stored predicate
`pred`.

### Parameters

- **pred** — predicate function object

## std::binary_negate::operator()

```cpp
bool operator()( first_argument_type const& x,
                 second_argument_type const& y ) const;  // (until C++14)
constexpr bool operator()( first_argument_type const& x,
                           second_argument_type const& y ) const;  // (since C++14)
```

Returns the logical complement of the result of calling `pred(x, y)`.

### Parameters

- **x** — first argument to pass through to predicate
- **y** — second argument to pass through to predicate

### Return value

The logical complement of the result of calling `pred(x, y)`.

### Example

```cpp
#include <algorithm>
#include <cstddef>
#include <functional>
#include <iostream>
#include <vector>

struct same : std::binary_function<int, int, bool>
{
    bool operator()(int a, int b) const { return a == b; }
};

int main()
{
    std::vector<int> v1;
    for (int i = 0; i < 7; ++i)
        v1.push_back(i);

    std::vector<int> v2(v1.size());
    std::reverse_copy(v1.begin(), v1.end(), v2.begin());

    std::vector<bool> v3(v1.size());

    std::binary_negate<same> not_same((same()));

    // C++11 solution:
    // std::function<bool (int, int)> not_same =
    //     [](int x, int y) -> bool { return !same()(x, y); };

    std::transform(v1.begin(), v1.end(), v2.begin(), v3.begin(), not_same);

    std::cout.setf(std::ios_base::boolalpha);
    for (std::size_t i = 0; i != v1.size(); ++i)
        std::cout << v1[i] << " != " << v2[i] << " : " << v3[i] << '\n';
}
```

Output:

```text
0 != 6 : true
1 != 5 : true
2 != 4 : true
3 != 3 : false
4 != 2 : true
5 != 1 : true
6 != 0 : true
```

### See also

- **binary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible binary function base class (class template)
- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **not2 (deprecated in C++17)(removed in C++20)** — constructs custom
  `std::binary_negate` object (function template)
- **ptr_fun (deprecated in C++11)(removed in C++17)** — creates an
  adaptor-compatible function object wrapper from a pointer to function
  (function template)
- **unary_negate (deprecated in C++17)(removed in C++20)** — wrapper function
  object returning the complement of the unary predicate it holds (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/binary_negate*
