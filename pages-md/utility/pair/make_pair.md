# std::make_pair

```cpp
template< class T1, class T2 >
std::pair<T1, T2> make_pair( T1 t, T2 u );  // (until C++11)
template< class T1, class T2 >
std::pair<V1, V2> make_pair( T1&& t, T2&& u );  // (since C++11) (until C++14)
template< class T1, class T2 >
constexpr std::pair<V1, V2> make_pair( T1&& t, T2&& u );  // (since C++14)
```

Creates a `std::pair` object, deducing the target type from the types of
arguments.

The deduced types `V1` and `V2` are `std::decay<T1>::type` and
`std::decay<T2>::type` (the usual type transformations applied to arguments of
functions passed by value) unless application of `std::decay` results in
`std::reference_wrapper<X>` for some type `X`, in which case the deduced type is
`X&`.
*(since C++11)*

### Parameters

- **t, u** — the values to construct the pair from

### Return value

A `std::pair` object containing the given values.

### Example

```cpp
#include <iostream>
#include <utility>
#include <functional>

int main()
{
    int n = 1;
    int a[5] = {1, 2, 3, 4, 5};

    // build a pair from two ints
    auto p1 = std::make_pair(n, a[1]);
    std::cout << "The value of p1 is "
              << "(" << p1.first << ", " << p1.second << ")\n";

    // build a pair from a reference to int and an array (decayed to pointer)
    auto p2 = std::make_pair(std::ref(n), a);
    n = 7;
    std::cout << "The value of p2 is "
              << "(" << p2.first << ", " << *(p2.second + 2) << ")\n";
}
```

Output:

```text
The value of p1 is (1, 2)
The value of p2 is (7, 3)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 181 | C++98 | the parameter types were const-reference types, which made
      passing arrays impossible | changed these types to value types

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/make_pair*
