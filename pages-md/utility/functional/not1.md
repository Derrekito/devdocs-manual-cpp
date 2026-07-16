# std::not1

```cpp
template< class Predicate >
std::unary_negate<Predicate> not1( const Predicate& pred );  // (until C++14)
template< class Predicate >
constexpr std::unary_negate<Predicate> not1( const Predicate& pred );  // (since C++14) (deprecated in C++17) (removed in C++20)
```

`std::not1` is a helper function to create a function object that returns the
complement of the unary predicate function passed. The function object created
is of type `std::unary_negate<Predicate>`.

The unary predicate type must define a member type, `argument_type`, that is
convertible to the predicate's parameter type. The unary function objects
obtained from `std::ref`, `std::cref`, `std::negate`, `std::logical_not`,
`std::mem_fn`, `std::function`, `std::hash`, or from another call to `std::not1`
have this type defined, as are function objects derived from the deprecated
`std::unary_function`.

### Parameters

- **pred** — unary predicate

### Return value

`std::not1` returns an object of type `std::unary_negate<Predicate>`,
constructed with `pred`.

### Exceptions

(none)

### Example

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

struct LessThan7 : std::unary_function<int, bool>
{
    bool operator()(int i) const { return i < 7; }
};

int main()
{
    std::vector<int> v(10);
    std::iota(std::begin(v), std::end(v), 0);

    std::cout << std::count_if(begin(v), end(v), std::not1(LessThan7())) << '\n';

    // the same as above using std::function
    std::function<bool(int)> less_than_9 = [](int x) { return x < 9; };
    std::cout << std::count_if(begin(v), end(v), std::not1(less_than_9)) << '\n';
}
```

Output:

```text
3
1
```

### See also

- **not_fn (C++17)** — creates a function object that returns the complement of
  the result of the function object it holds (function template)
- **unary_negate (deprecated in C++17)(removed in C++20)** — wrapper function
  object returning the complement of the unary predicate it holds (class
  template)
- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **not2 (deprecated in C++17)(removed in C++20)** — constructs custom
  `std::binary_negate` object (function template)
- **ptr_fun (deprecated in C++11)(removed in C++17)** — creates an
  adaptor-compatible function object wrapper from a pointer to function
  (function template)
- **unary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible unary function base class (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/not1*
