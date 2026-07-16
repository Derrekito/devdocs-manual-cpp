# deduction guides for `std::pair`

```cpp
template<class T1, class T2>
pair(T1, T2) -> pair<T1, T2>;  // (since C++17)
```

One deduction guide is provided for `std::pair` to account for the edge cases
missed by the implicit deduction guides, in particular, non-copyable arguments
and array to pointer conversion.

### Example

```cpp
#include <utility>

int main()
{
    int a[2], b[3];
    std::pair p{a, b}; // explicit deduction guide is used in this case
}
```

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/deduction_guides*
