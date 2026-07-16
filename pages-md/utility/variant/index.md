# std::variant<Types...>::index

```cpp
constexpr std::size_t index() const noexcept;  // (since C++17)
```

Returns the zero-based index of the alternative that is currently held by the
variant.

If the variant is valueless_by_exception, returns variant_npos.

### Example

```cpp
#include <variant>
#include <string>
#include <iostream>
int main()
{
    std::variant<int, std::string> v = "abc";

    std::cout << "v.index = " << v.index() << '\n';

    v = {};

    std::cout << "v.index = " << v.index() << '\n';
}
```

Output:

```text
v.index = 1
v.index = 0
```

### See also

- **holds_alternative (C++17)** — checks if a variant currently holds a given
  type (function template)
- **get(std::variant) (C++17)** — reads the value of the variant given the index
  or the type (if the type is unique), throws on error (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/index*
