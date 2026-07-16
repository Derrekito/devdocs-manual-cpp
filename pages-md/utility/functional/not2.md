# std::not2

```cpp
template< class Predicate >
std::binary_negate<Predicate> not2( const Predicate& pred );  // (until C++14)
template< class Predicate >
constexpr std::binary_negate<Predicate> not2( const Predicate& pred );  // (since C++14) (deprecated in C++17) (removed in C++20)
```

`std::not2` is a helper function to create a function object that returns the
complement of the binary predicate function passed. The function object created
is of type `std::binary_negate<Predicate>`.

The binary predicate type must define two member types, `first_argument_type`
and `second_argument_type`, that are convertible to the predicate's parameter
types. The function objects obtained from `std::owner_less`, `std::ref`,
`std::cref`, `std::plus`, `std::minus`, `std::multiplies`, `std::divides`,
`std::modulus`, `std::equal_to`, `std::not_equal_to`, `std::greater`,
`std::less`, `std::greater_equal`, `std::less_equal`, `std::logical_not`,
`std::logical_or`, `std::bit_and`, `std::bit_or`, `std::bit_xor`, `std::mem_fn`,
`std::map::value_comp`, `std::multimap::value_comp`, `std::function`, or from
another call to `std::not2` have these types defined, as are function objects
derived from the deprecated `std::binary_function`.

### Parameters

- **pred** — binary predicate

### Return value

`std::not2` returns an object of type `std::binary_negate<Predicate>`,
constructed with `pred`.

### Exceptions

(none)

### Example

```cpp
#include <algorithm>
#include <cstddef>
#include <functional>
#include <iostream>
#include <vector>

struct old_same : std::binary_function<int, int, bool>
{
    bool operator()(int a, int b) const { return a == b; }
};

struct new_same
{
    bool operator()(int a, int b) const { return a == b; }
};

bool same_fn(int a, int b)
{
    return a == b;
}

int main()
{
    std::vector<int> v1{0, 1, 2};
    std::vector<int> v2{2, 1, 0};
    std::vector<bool> v3(v1.size());

    std::cout << "negating a binary_function:\n";
    std::transform(v1.begin(), v1.end(), v2.begin(), v3.begin(),
                   std::not2(old_same()));

    std::cout << std::boolalpha;
    for (std::size_t i = 0; i < v1.size(); ++i)
        std::cout << v1[i] << ' ' << v2[i] << ' ' << v3[i] << '\n';

    std::cout << "negating a standard functor:\n";
    std::transform(v1.begin(), v1.end(), v2.begin(), v3.begin(),
                   std::not2(std::equal_to<int>()));

    for (std::size_t i = 0; i < v1.size(); ++i)
        std::cout << v1[i] << ' ' << v2[i] << ' ' << v3[i] << '\n';

    std::cout << "negating a std::function:\n";
    std::transform(v1.begin(), v1.end(), v2.begin(), v3.begin(),
                   std::not2(std::function<bool(int, int)>(new_same())));

    for (std::size_t i = 0; i < v1.size(); ++i)
        std::cout << v1[i] << ' ' << v2[i] << ' ' << v3[i] << '\n';

    std::cout << "negating a std::reference_wrapper:\n";
    std::transform(v1.begin(), v1.end(), v2.begin(), v3.begin(),
                   std::not2(std::ref(same_fn)));

    for (std::size_t i = 0; i < v1.size(); ++i)
        std::cout << v1[i] << ' ' << v2[i] << ' ' << v3[i] << '\n';
}
```

Output:

```text
negating a binary_function:
0 2 true
1 1 false
2 0 true
negating a standard functor:
0 2 true
1 1 false
2 0 true
negating a std::function:
0 2 true
1 1 false
2 0 true
negating a std::reference_wrapper:
0 2 true
1 1 false
2 0 true
```

### See also

- **not_fn (C++17)** — creates a function object that returns the complement of
  the result of the function object it holds (function template)
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
- **binary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible binary function base class (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/not2*
