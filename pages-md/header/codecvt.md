# Standard library header <codecvt> (C++11)(deprecated in C++17)(removed in C++26)

This header is part of the localization library.

**Classes**

- **codecvt_utf8 (C++11)(deprecated in C++17)(removed in C++26)** — converts
  between UTF-8 and UCS-2/UCS-4 (class template)
- **codecvt_utf16 (C++11)(deprecated in C++17)(removed in C++26)** — converts
  between UTF-16 and UCS-2/UCS-4 (class template)
- **codecvt_utf8_utf16 (C++11)(deprecated in C++17)(removed in C++26)** —
  converts between UTF-8 and UTF-16 (class template)
- **codecvt_mode (C++11)(deprecated in C++17)(removed in C++26)** — tags to
  alter behavior of the standard codecvt facets (enum)

### Notes

`<codecvt>` is deprecated in C++17 and removed in C++26.

The reason for removal is that this feature no longer implements the current
Unicode Standard, supporting only the obsolete UCS-2 encoding.

### Synopsis

```cpp
namespace std {
  enum codecvt_mode {
    consume_header = 4,
    generate_header = 2,
    little_endian = 1
  };

  template<class Elem, unsigned long Maxcode = 0x10ffff,
           codecvt_mode Mode = (codecvt_mode)0>
    class codecvt_utf8;

  template<class Elem, unsigned long Maxcode = 0x10ffff,
           codecvt_mode Mode = (codecvt_mode)0>
    class codecvt_utf16;

  template<class Elem, unsigned long Maxcode = 0x10ffff,
           codecvt_mode Mode = (codecvt_mode)0>
    class codecvt_utf8_utf16;
}
```

#### Class `std::codecvt_utf8`

```cpp
namespace std {
  template<class Elem, unsigned long Maxcode = 0x10ffff,
           codecvt_mode Mode = (codecvt_mode)0>
    class codecvt_utf8 : public codecvt<Elem, char, mbstate_t> {
    public:
      explicit codecvt_utf8(size_t refs = 0);
      ~codecvt_utf8();
    };
}
```

#### Class `std::codecvt_utf16`

```cpp
namespace std {
  template<class Elem, unsigned long Maxcode = 0x10ffff,
           codecvt_mode Mode = (codecvt_mode)0>
    class codecvt_utf16 : public codecvt<Elem, char, mbstate_t> {
    public:
      explicit codecvt_utf16(size_t refs = 0);
      ~codecvt_utf16();
    };
}
```

#### Class `std::codecvt_utf8_utf16`

```cpp
namespace std {
  template<class Elem, unsigned long Maxcode = 0x10ffff,
           codecvt_mode Mode = (codecvt_mode)0>
    class codecvt_utf8_utf16 : public codecvt<Elem, char, mbstate_t> {
    public:
      explicit codecvt_utf8_utf16(size_t refs = 0);
      ~codecvt_utf8_utf16();
    };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/codecvt*
