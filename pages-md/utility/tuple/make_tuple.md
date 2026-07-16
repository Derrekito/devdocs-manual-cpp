# std::make_tuple

```cpp
template< class... Types >
std::tuple<VTypes...> make_tuple( Types&&... args );  // (since C++11) (constexpr since C++14)
```

Creates a tuple object, deducing the target type from the types of arguments.

For each `Ti` in `Types...`, the corresponding type `Vi` in `VTypes...` is
`std::decay<Ti>::type` unless application of `std::decay` results in
`std::reference_wrapper<X>` for some type `X`, in which case the deduced type is
`X&`.

### Parameters

- **args** — zero or more arguments to construct the tuple from

### Return value

A `std::tuple` object containing the given values, created as if by
`std::tuple<VTypes...>(std::forward<Types>(t)...).`

### Possible implementation

```cpp
template <class T>
struct unwrap_refwrapper
{
    using type = T;
};

template <class T>
struct unwrap_refwrapper<std::reference_wrapper<T>>
{
    using type = T&;
};

template <class T>
using unwrap_decay_t = typename unwrap_refwrapper<typename std::decay<T>::type>::type;
// or use std::unwrap_ref_decay_t (since C++20)

template <class... Types>
constexpr // since C++14
std::tuple<unwrap_decay_t<Types>...> make_tuple(Types&&... args)
{
    return std::tuple<unwrap_decay_t<Types>...>(std::forward<Types>(args)...);
}
```

### Example

```cpp
#include <iostream>
#include <tuple>
#include <functional>

std::tuple<int, int> f() // this function returns multiple values
{
    int x = 5;
    return std::make_tuple(x, 7); // return {x,7}; in C++17
}

int main()
{
    // heterogeneous tuple construction
    int n = 1;
    auto t = std::make_tuple(10, "Test", 3.14, std::ref(n), n);
    n = 7;
    std::cout << "The value of t is ("
              << std::get<0>(t) << ", "
              << std::get<1>(t) << ", "
              << std::get<2>(t) << ", "
              << std::get<3>(t) << ", "
              << std::get<4>(t) << ")\n";

    // function returning multiple values
    int a, b;
    std::tie(a, b) = f();
    std::cout << a << ' ' << b << '\n';
}
```

Output:

```text
The value of t is (10, Test, 3.14, 7, 1)
5 7
```

### See also

- **tie (C++11)** — creates a `tuple` of lvalue references or unpacks a tuple
  into individual objects (function template)
- **forward_as_tuple (C++11)** — creates a `tuple` of forwarding references
  (function template)
- **tuple_cat (C++11)** — creates a `tuple` by concatenating any number of
  tuples (function template)
- **apply (C++17)** — calls a function with a tuple of arguments (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/make_tuple*
