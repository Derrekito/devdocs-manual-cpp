# std::endian

```cpp
enum class endian
{
    little = /*implementation-defined*/,
    big    = /*implementation-defined*/,
    native = /*implementation-defined*/,
};  // (since C++20)
```

Indicates the endianness of all scalar types:

- If all scalar types are little-endian, `std::endian::native` equals
  `std::endian::little`.
- If all scalar types are big-endian, `std::endian::native` equals
  `std::endian::big`.

Corner case platforms are also supported:

- If all scalar types have `sizeof` equal to `1`, endianness does not matter and
  all three values, `std::endian::little`, `std::endian::big`, and
  `std::endian::native` are the same.
- If the platform uses mixed endian, `std::endian::native` equals neither
  `std::endian::big` nor `std::endian::little`.

### Possible implementation

```cpp
enum class endian
{
#if defined(_MSC_VER) && !defined(__clang__)
    little = 0,
    big    = 1,
    native = little
#else
    little = __ORDER_LITTLE_ENDIAN__,
    big    = __ORDER_BIG_ENDIAN__,
    native = __BYTE_ORDER__
#endif
};
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_endian` | 201907L | (C++20) | `std::endian`

### Example

```cpp
#include <bit>
#include <iostream>

int main()
{
    if constexpr (std::endian::native == std::endian::big)
        std::cout << "big-endian\n";
    else if constexpr (std::endian::native == std::endian::little)
        std::cout << "little-endian\n";
    else
        std::cout << "mixed-endian\n";
}
```

Possible output:

```text
mixed-endian
```

### See also

- **byteswap (C++23)** — reverses the bytes in the given integer value (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/endian*
