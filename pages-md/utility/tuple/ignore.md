# std::ignore

```cpp
const /*unspecified*/ ignore;  // (since C++11) (until C++17)
inline constexpr /*unspecified*/ ignore;  // (since C++17)
```

An object of unspecified type such that any value can be assigned to it with no
effect. Intended for use with `std::tie` when unpacking a `std::tuple`, as a
placeholder for the arguments that are not used.

While the behavior of `std::ignore` outside of `std::tie` is not formally
specified, some code guides recommend using `std::ignore` to avoid warnings from
unused return values of `[[nodiscard]]` functions.

### Possible implementation

```cpp
namespace detail {
struct ignore_t
{
    template <typename T>
    constexpr // required since C++14
    void operator=(T&&) const noexcept {}
};
}
inline constexpr detail::ignore_t ignore; // 'const' only until C++17
```

### Example

1. Demonstrates the use of `std::ignore` together with a `[[nodiscard]]`
   function.
1. Unpacks a `std::pair<iterator, bool>` returned by `std::set::insert()`, but
   only saves the boolean.

```cpp
#include <iostream>
#include <set>
#include <string>
#include <tuple>

[[nodiscard]] int dontIgnoreMe()
{
    return 42;
}

int main()
{
    std::ignore = dontIgnoreMe();

    std::set<std::string> set_of_str;
    bool inserted = false;
    std::tie(std::ignore, inserted) = set_of_str.insert("Test");
    if (inserted)
        std::cout << "Value was inserted successfully\n";
}
```

Output:

```text
Value was inserted successfully
```

### See also

- **tie (C++11)** — creates a `tuple` of lvalue references or unpacks a tuple
  into individual objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/ignore*
