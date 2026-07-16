# Standard library header <cstddef>

This header was originally in the C standard library as `<stddef.h>`.

This header is part of the utility library.

**Macros**

- **NULL** — implementation-defined null pointer constant (macro constant)
- **offsetof** — byte offset from the beginning of a standard-layout type to
  specified member (function macro)

**Types**

- **size_t** — unsigned integer type returned by the `sizeof` operator (typedef)
- **ptrdiff_t** — signed integer type returned when subtracting two pointers
  (typedef)
- **nullptr_t (C++11)** — the type of the null pointer literal `nullptr`
  (typedef)
- **max_align_t (C++11)** — trivial type with alignment requirement as great as
  any other scalar type (typedef)
- **byte (C++17)** — the byte type (enum)

**Functions**

- **to_integer (C++17)** — convert `std::byte` to integer (function template)

### Synopsis

```cpp
namespace std {
  using ptrdiff_t = /* see description */;
  using size_t = /* see description */;
  using max_align_t = /* see description */;
  using nullptr_t = decltype(nullptr);

  enum class byte : unsigned char {};

  // byte type operations
  template<class IntType>
    constexpr byte& operator<<=(byte& b, IntType shift) noexcept;
  template<class IntType>
    constexpr byte operator<<(byte b, IntType shift) noexcept;
  template<class IntType>
    constexpr byte& operator>>=(byte& b, IntType shift) noexcept;
  template<class IntType>
    constexpr byte operator>>(byte b, IntType shift) noexcept;
  constexpr byte& operator|=(byte& l, byte r) noexcept;
  constexpr byte operator|(byte l, byte r) noexcept;
  constexpr byte& operator&=(byte& l, byte r) noexcept;
  constexpr byte operator&(byte l, byte r) noexcept;
  constexpr byte& operator^=(byte& l, byte r) noexcept;
  constexpr byte operator^(byte l, byte r) noexcept;
  constexpr byte operator~(byte b) noexcept;
  template<class IntType>
    constexpr IntType to_integer(byte b) noexcept;
}

#define NULL /* see description */
#define offsetof(P, D) /* see description */
```

### Notes

- `NULL` is also defined in the following headers: `<clocale>` `<ctime>`
  `<cstring>` `<cstdio>` `<cstdlib>` `<cwchar>`
- `std::size_t` is also defined in the following headers: `<ctime>` `<cstring>`
  `<cstdio>` `<cwchar>` `<cuchar>` (since C++17)

---
*Source: https://en.cppreference.com/w/cpp/header/cstddef*
