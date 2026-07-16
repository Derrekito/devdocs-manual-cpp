# deduction guides for `std::optional`

```cpp
template< class T >
optional(T) -> optional<T>;  // (since C++17)
```

One deduction guide is provided for `std::optional` to account for the edge
cases missed by the implicit deduction guides, in particular, non-copyable
arguments and array to pointer conversion.

### Example

```cpp
#include <optional>
#include <type_traits>

int main()
{
    int a[2];
    std::optional oa{a}; // uses explicit deduction guide
    static_assert(std::is_same_v<decltype(oa), std::optional<int*>> == true);
}
```

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/deduction_guides*
