# std::make_unsigned

```cpp
template< class T >
struct make_unsigned;  // (since C++11)
```

If `T` is an integral (except bool) or enumeration type, provides the member
typedef `type` which is the unsigned integer type corresponding to `T`, with the
same cv-qualifiers.

If `T` is signed or unsigned char, short, int, long, long long; the unsigned
type from this list corresponding to `T` is provided.

If `T` is an enumeration type or char, wchar_t, `char8_t`(since C++20),
char16_t, char32_t; the unsigned integer type with the smallest rank having the
same `sizeof` as `T` is provided.

Otherwise, the behavior is undefined.
*(until C++20)*

Otherwise, the program is ill-formed.
*(since C++20)*

The behavior of a program that adds specializations for `std::make_unsigned` is
undefined.

### Member types

- **`type`** — the unsigned integer type corresponding to `T`

### Helper types

```cpp
template< class T >
using make_unsigned_t = typename make_unsigned<T>::type;  // (since C++14)
```

### Example

```cpp
#include <type_traits>

int main()
{
    using uchar_type = std::make_unsigned_t<char>;
    using uint_type  = std::make_unsigned_t<int>;
    using ulong_type = std::make_unsigned_t<volatile long>;

    static_assert(
        std::is_same_v<uchar_type, unsigned char> and
        std::is_same_v<uint_type, unsigned int> and
        std::is_same_v<ulong_type, volatile unsigned long>
    );
}
```

### See also

- **is_signed (C++11)** — checks if a type is a signed arithmetic type (class
  template)
- **is_unsigned (C++11)** — checks if a type is an unsigned arithmetic type
  (class template)
- **make_signed (C++11)** — makes the given integral type signed (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/make_unsigned*
