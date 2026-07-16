# std::holds_alternative

```cpp
template< class T, class... Types >
constexpr bool holds_alternative( const std::variant<Types...>& v ) noexcept;  // (since C++17)
```

Checks if the variant `v` holds the alternative `T`. The call is ill-formed if
`T` does not appear exactly once in `Types...`

### Parameters

- **v** — variant to examine

### Return value

`true` if the variant currently holds the alternative `T`, `false` otherwise.

### Example

```cpp
#include <variant>
#include <string>
#include <iostream>
int main()
{
    std::variant<int, std::string> v = "abc";
    std::cout << std::boolalpha
              << "variant holds int? "
              << std::holds_alternative<int>(v) << '\n'
              << "variant holds string? "
              << std::holds_alternative<std::string>(v) << '\n';
}
```

Output:

```text
variant holds int? false
variant holds string? true
```

### See also

- **index** — returns the zero-based index of the alternative held by the
  variant (public member function)
- **get(std::variant) (C++17)** — reads the value of the variant given the index
  or the type (if the type is unique), throws on error (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/holds_alternative*
