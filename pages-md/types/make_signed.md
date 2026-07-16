# std::make_signed

```cpp
template< class T >
struct make_signed;  // (since C++11)
```

If `T` is an integral (except bool) or enumeration type, provides the member
typedef `type` which is the signed integer type corresponding to `T`, with the
same cv-qualifiers.

If `T` is signed or unsigned char, short, int, long, long long, the signed type
from this list corresponding to `T` is provided.

If `T` is an enumeration type or char, wchar_t, `char8_t`(since C++20),
char16_t, char32_t, the signed integer type with the smallest rank having the
same `sizeof` as `T` is provided.

Otherwise, the behavior is undefined.
*(until C++20)*

Otherwise, the program is ill-formed.
*(since C++20)*

The behavior of a program that adds specializations for `std::make_signed` is
undefined.

### Member types

- **`type`** — the signed integer type corresponding to `T`

### Helper types

```cpp
template< class T >
using make_signed_t = typename make_signed<T>::type;  // (since C++14)
```

### Example

```cpp
#include <type_traits>

enum struct E : unsigned short {};

int main()
{
    using char_type = std::make_signed_t<unsigned char>;
    using int_type  = std::make_signed_t<unsigned int>;
    using long_type = std::make_signed_t<volatile unsigned long>;
    using enum_type = std::make_signed_t<E>;

    static_assert(
        std::is_same_v<char_type, signed char> and
        std::is_same_v<int_type, signed int> and
        std::is_same_v<long_type, volatile signed long> and
        std::is_same_v<enum_type, signed short>
    );
}
```

### See also

- **is_signed (C++11)** — checks if a type is a signed arithmetic type (class
  template)
- **is_unsigned (C++11)** — checks if a type is an unsigned arithmetic type
  (class template)
- **make_unsigned (C++11)** — makes the given integral type unsigned (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/make_signed*
