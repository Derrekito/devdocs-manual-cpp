# std::unreachable

```cpp
[[noreturn]] void unreachable();  // (since C++23)
```

Invokes undefined behavior. An implementation may use this to optimize
impossible code branches away (typically, in optimized builds) or to trap them
to prevent further execution (typically, in debug builds).

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_unreachable` | 202202L | (C++23) | `std::unreachable`

### Possible implementation

```cpp
[[noreturn]] inline void unreachable()
{
    // Uses compiler specific extensions if possible.
    // Even if no extension is used, undefined behavior is still raised by
    // an empty function body and the noreturn attribute.
#if defined(_MSC_VER) && !defined(__clang__) // MSVC
    __assume(false);
#else // GCC, Clang
    __builtin_unreachable();
#endif
}
```

### Example

```cpp
#include <cassert>
#include <cstddef>
#include <cstdint>
#include <utility>
#include <vector>

struct Color { std::uint8_t r, g, b, a; };

// Assume that only restricted set of texture caps is supported.
void generate_texture(std::vector<Color>& tex, std::size_t xy)
{
    switch (xy)
    {
    case 128: [[fallthrough]];
    case 256: [[fallthrough]];
    case 512: /* ... */
        tex.clear();
        tex.resize(xy * xy, Color{0, 0, 0, 0});
        break;
    default:
        std::unreachable();
    }
}

int main()
{
    std::vector<Color> tex;
    generate_texture(tex, 128); // OK
    assert(tex.size() == 128 * 128);
    generate_texture(tex, 32);  // Results in undefined behavior
}
```

Possible output:

```text
Segmentation fault
```

### See also

- **`[[assume(expression)]]`(C++23)** — specifies that the *expression* will
  always evaluate to true at a given point (attribute specifier)
- **assume_aligned (C++20)** — informs the compiler that a pointer is aligned
  (function template)

**C documentation for `unreachable`**

### External Links

  1. | GCC docs: `__builtin_unreachable`
  2. | Clang docs: `__builtin_unreachable`
  3. | MSVC docs: `__assume`

---
*Source: https://en.cppreference.com/w/cpp/utility/unreachable*
