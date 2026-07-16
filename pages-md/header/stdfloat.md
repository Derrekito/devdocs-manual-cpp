# Standard library header <stdfloat> (C++23)

This header is part of the type support library, providing fixed width
floating-point types.

**Types**

- **float16_tfloat32_tfloat64_tfloat128_t (C++23)(optional)** — binary
  floating-point type with width of exactly 16, 32, 64, and 128 bits
  respectively (typedef)
- **bfloat16_t (C++23)(optional)** — brain floating-point type with 16 bits
  exactly (typedef)

### Synopsis

```cpp
namespace std {
  #if defined(__STDCPP_FLOAT16_T__)
    using float16_t  = /* implementation-defined */;
  #endif
  #if defined(__STDCPP_FLOAT32_T__)
    using float32_t  = /* implementation-defined */;
  #endif
  #if defined(__STDCPP_FLOAT64_T__)
    using float64_t  = /* implementation-defined */;
  #endif
  #if defined(__STDCPP_FLOAT128_T__)
    using float128_t = /* implementation-defined */;
  #endif
  #if defined(__STDCPP_BFLOAT16_T__)
    using bfloat16_t = /* implementation-defined */;
  #endif
}
```

### References

- C++23 standard (ISO/IEC 14882:2023):

---
*Source: https://en.cppreference.com/w/cpp/header/stdfloat*
