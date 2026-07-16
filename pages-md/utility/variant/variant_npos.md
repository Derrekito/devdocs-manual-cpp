# std::variant_npos

```cpp
inline constexpr std::size_t variant_npos = -1;  // (since C++17)
```

This is a special value equal to the largest value representable by the type
`std::size_t`, used as the return value of `index()` when
`valueless_by_exception()` is `true`

```cpp
#include <iostream>
#include <stdexcept>
#include <string>
#include <variant>

struct Demon
{
    Demon(int) {}
    Demon(const Demon&) { throw std::domain_error("copy ctor"); }
    Demon& operator= (const Demon&) = default;
};

int main()
{
    std::variant<int, Demon> var{42};
    std::cout
        << std::boolalpha
        << "index == npos: " << (var.index() == std::variant_npos) << '\n';

    try { var = Demon{666}; } catch (const std::domain_error& ex)
    {
        std::cout
            << "Exception: " << ex.what() << '\n'
            << "index == npos: " << (var.index() == std::variant_npos) << '\n'
            << "valueless: " << var.valueless_by_exception() << '\n';
    }
}
```

Possible output:

```text
index == npos: false
Exception: copy ctor
index == npos: true
valueless: true
```

### See also

- **index** — returns the zero-based index of the alternative held by the
  variant (public member function)
- **valueless_by_exception** — checks if the variant is in the invalid state
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/variant_npos*
