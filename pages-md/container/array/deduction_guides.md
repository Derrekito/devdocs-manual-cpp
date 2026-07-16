# deduction guides for `std::array`

```cpp
template< class T, class... U >
array( T, U... )
  -> array<T, 1 + sizeof...(U)>;  // (since C++17)
```

One deduction guide is provided for `std::array` to provide an equivalent of
`std::experimental::make_array` for construction of std::array from a variadic
parameter pack.

The program is ill-formed if `(std::is_same_v<T, U> && ...)` is not true. Note
that `(std::is_same_v<T, U> && ...)` is true when `sizeof...(U)` is zero.

### Example

```cpp
#include <array>
#include <cassert>

int main()
{
    int const x = 10;
    std::array a{1, 2, 3, 5, x}; // OK, creates std::array<int, 5>
    assert(a.back() == x);

//  std::array b{1, 2u}; // Error, all arguments must have the same type

//  std::array<short> c{3, 2, 1}; // Error, wrong number of template args
    std::array c(std::to_array<short>({3, 2, 1})); // C++20 alternative,
                                                   // creates std::array<short, 3>
}
```

---
*Source: https://en.cppreference.com/w/cpp/container/array/deduction_guides*
