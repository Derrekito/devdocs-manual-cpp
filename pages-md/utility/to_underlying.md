# std::to_underlying

```cpp
template< class Enum >
constexpr std::underlying_type_t<Enum> to_underlying( Enum e ) noexcept;  // (since C++23)
```

Converts an enumeration to its underlying type. Equivalent to `return
static_cast<std::underlying_type_t<Enum>>(e);`.

### Parameters

- **e** — enumeration value to convert

### Return value

The integer value of the underlying type of `Enum`, converted from `e`.

### Notes

`std::to_underlying` can be used to avoid converting an enumeration to an
integer type other than its underlying type.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_to_underlying` | 202102L | (C++23) | `std::to_underlying`

### Example

```cpp
#include <cstdint>
#include <iomanip>
#include <iostream>
#include <type_traits>
#include <utility>

enum class E1 : char { e };
static_assert(std::is_same_v<char, decltype(std::to_underlying(E1::e))>);

enum struct E2 : long { e };
static_assert(std::is_same_v<long, decltype(std::to_underlying(E2::e))>);

enum E3 : unsigned { e };
static_assert(std::is_same_v<unsigned, decltype(std::to_underlying(e))>);

int main()
{
    enum class ColorMask : std::uint32_t
    {
        red = 0xFF, green = (red << 8), blue = (green << 8), alpha = (blue << 8)
    };

    std::cout << std::hex << std::uppercase << std::setfill('0')
              << std::setw(8) << std::to_underlying(ColorMask::red) << '\n'
              << std::setw(8) << std::to_underlying(ColorMask::green) << '\n'
              << std::setw(8) << std::to_underlying(ColorMask::blue) << '\n'
              << std::setw(8) << std::to_underlying(ColorMask::alpha) << '\n';

//  std::underlying_type_t<ColorMask> x = ColorMask::alpha; // Error: no known conversion
    [[maybe_unused]]
    std::underlying_type_t<ColorMask> y = std::to_underlying(ColorMask::alpha); // OK
}
```

Output:

```text
000000FF
0000FF00
00FF0000
FF000000
```

### See also

- **underlying_type (C++11)** — obtains the underlying integer type for a given
  enumeration type (class template)
- **is_enum (C++11)** — checks if a type is an enumeration type (class template)
- **is_scoped_enum (C++23)** — checks if a type is a scoped enumeration type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/to_underlying*
