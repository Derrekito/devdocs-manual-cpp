# Standard library header <print> (C++23)

This header is part of the Input/Output library.

**Functions**

- **print (C++23)** — prints to `stdout` or a file stream using formatted
  representation of the arguments (function template)
- **println (C++23)** — same as `std::print` except that each print is
  terminated by additional new line (function template)
- **vprint_unicode (C++23)** — prints to Unicode capable `stdout` or a file
  stream using type-erased argument representation (function)
- **vprint_nonunicode (C++23)** — prints to `stdout` or a file stream using
  type-erased argument representation (function)

### Synopsis

```cpp
namespace std {
  // print functions
  template<class... Args>
    void print(format_string<Args...> fmt, Args&&... args);
  template<class... Args>
    void print(FILE* stream, format_string<Args...> fmt, Args&&... args);

  template<class... Args>
    void println(format_string<Args...> fmt, Args&&... args);
  template<class... Args>
    void println(FILE* stream, format_string<Args...> fmt, Args&&... args);

  void vprint_unicode(string_view fmt, format_args args);
  void vprint_unicode(FILE* stream, string_view fmt, format_args args);

  void vprint_nonunicode(string_view fmt, format_args args);
  void vprint_nonunicode(FILE* stream, string_view fmt, format_args args);
}
```

### References

- C++23 standard (ISO/IEC 14882:2023):

---
*Source: https://en.cppreference.com/w/cpp/header/print*
