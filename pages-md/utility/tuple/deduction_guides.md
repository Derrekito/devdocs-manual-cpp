# deduction guides for `std::tuple`

```cpp
template<class... UTypes>
tuple(UTypes...) -> tuple<UTypes...>;  // (1) (since C++17)
template<class T1, class T2>
tuple(std::pair<T1, T2>) -> tuple<T1, T2>;  // (2) (since C++17)
template<class Alloc, class... UTypes>
tuple(std::allocator_arg_t, Alloc, UTypes...) -> tuple<UTypes...>;  // (3) (since C++17)
template<class Alloc, class T1, class T2>
tuple(std::allocator_arg_t, Alloc, std::pair<T1, T2>) -> tuple<T1, T2>;  // (4) (since C++17)
template<class Alloc, class... UTypes>
tuple(std::allocator_arg_t, Alloc, tuple<UTypes...>) -> tuple<UTypes...>;  // (5) (since C++17)
```

These deduction guides are provided for `std::tuple` to account for the edge
cases missed by the implicit deduction guides, in particular, non-copyable
arguments and array to pointer conversion.

### Example

```cpp
#include <tuple>
int main()
{
    int a[2], b[3], c[4];
    std::tuple t1{a, b, c}; // explicit deduction guide is used in this case
}
```

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/deduction_guides*
