# Standard library header <charconv> (C++17)

This header is part of the strings library.

**Classes**

- **chars_format (C++17)** — specifies formatting for `std::to_chars` and
  `std::from_chars` (enum)
- **from_chars_result (C++17)** — the return type of `std::from_chars` (class)
- **to_chars_result (C++17)** — the return type of `std::to_chars` (class)

**Functions**

- **from_chars (C++17)** — converts a character sequence to an integer or
  floating-point value (function)
- **to_chars (C++17)** — converts an integer or floating-point value to a
  character sequence (function)

### Synopsis

```cpp
namespace std {
  // floating-point format for primitive numerical conversion
  enum class chars_format {
    scientific = /* unspecified */,
    fixed = /* unspecified */,
    hex = /* unspecified */,
    general = fixed | scientific
  };

  // primitive numerical output conversion
  struct to_chars_result { // freestanding
    char* ptr;
    errc ec;
    friend bool operator==(const to_chars_result&, const to_chars_result&) = default;
    constexpr explicit operator bool() const noexcept { return ec == errc{}; }
  };

  constexpr
  to_chars_result to_chars(char* first, char* last, // freestanding
                           /* integer-type */ value, int base = 10);
  to_chars_result to_chars(char* first, char* last, // freestanding
                           bool value, int base = 10) = delete;
  to_chars_result to_chars(char* first, char* last, // freestanding-deleted
                           /* floating-point-type */ value);
  to_chars_result to_chars(char* first, char* last, // freestanding-deleted
                           /* floating-point-type */ value, chars_format fmt);
  to_chars_result to_chars(char* first, char* last, // freestanding-deleted
                           /* floating-point-type */ value,
                           chars_format fmt, int precision);

  // primitive numerical input conversion
  struct from_chars_result { // freestanding
    const char* ptr;
    errc ec;
    friend bool operator==(const from_chars_result&, const from_chars_result&) = default;
    constexpr explicit operator bool() const noexcept { return ec == errc{}; }
  };

  constexpr
  from_chars_result from_chars(const char* first, // freestanding
                               const char* last, /* integer-type */& value,
                               int base = 10);
  from_chars_result from_chars(const char* first, // freestanding-deleted
                               const char* last, /* floating-point-type */& value,
                               chars_format fmt = chars_format::general);
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/charconv*
