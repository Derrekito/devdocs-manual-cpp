# std::ctype_base

```cpp
class ctype_base;
```

The class `std::ctype_base` lists the character classification categories which
are inherited by the `std::ctype` facets.

### Member types

- **mask** — unspecified BitmaskType (enumeration, integer type, or bitset)
  (typedef)
- **space [static]** — the value of `mask` identifying whitespace character
  classification (public static member constant)
- **print [static]** — the value of `mask` identifying printable character
  classification (public static member constant)
- **cntrl [static]** — the value of `mask` identifying control character
  classification (public static member constant)
- **upper [static]** — the value of `mask` identifying uppercase character
  classification (public static member constant)
- **lower [static]** — the value of `mask` identifying lowercase character
  classification (public static member constant)
- **alpha [static]** — the value of `mask` identifying alphabetic character
  classification (public static member constant)
- **digit [static]** — the value of `mask` identifying digit character
  classification (public static member constant)
- **punct [static]** — the value of `mask` identifying punctuation character
  classification (public static member constant)
- **xdigit [static]** — the value of `mask` identifying hexadecimal digit
  character classification (public static member constant)
- **blank [static] (C++11)** — the value of `mask` identifying blank character
  classification (public static member constant)
- **alnum [static]** — `alpha | digit` (public static member constant)
- **graph [static]** — `alnum | punct` (public static member constant)
- **ctype** — defines character classification tables (class template)
- **ctype<char>** — specialization of `std::ctype` for type char (class template
  specialization)
- **ctype_byname** — represents the system-supplied `std::ctype` for the named
  locale (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/ctype_base*
