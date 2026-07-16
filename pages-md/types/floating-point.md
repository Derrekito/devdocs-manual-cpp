# Fixed width floating-point types (since C++23)

If the implementation supports any of the following ISO 60559 types as an
extended floating point type, then:

- the corresponding macro is defined as 1 to indicate support,
- the corresponding floating-point literal suffix is available, and
- the corresponding type alias name is provided:

  **Type name** Defined in header `<stdfloat>` | Literal suffix | Predefined
      macro | C language type | Type properties
  bits of storage | bits of precision | bits of exponent | max exponent
  `std::float16_t` | `f16` or `F16` | `__STDCPP_FLOAT16_T__` | `_Float16` | 16 |
      11 | 5 | 15
  `std::float32_t` | `f32` or `F32` | `__STDCPP_FLOAT32_T__` | `_Float32` | 32 |
      24 | 8 | 127
  `std::float64_t` | `f64` or `F64` | `__STDCPP_FLOAT64_T__` | `_Float64` | 64 |
      53 | 11 | 1023
  `std::float128_t` | `f128` or `F128` | `__STDCPP_FLOAT128_T__` | `_Float128` |
      128 | 113 | 15 | 16383
  `std::bfloat16_t` | `bf16` or `BF16` | `__STDCPP_BFLOAT16_T__` | (N/A) | 16 |
      8 | 8 | 127

### Notes

The type `std::bfloat16_t` is known as Brain Floating Point.

Unlike the fixed width integer types, which may be aliases to standard integer
types, the fixed width floating-point types must be aliases to extended floating
point types (not float / double / long double).

### Example

```cpp
#include <stdfloat>

#if __STDCPP_FLOAT64_T__ != 1
    #error "64-bit float type required"
#endif

int main()
{
    std::float64_t f = 0.1f64;
}
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- Fundamental types

---
*Source: https://en.cppreference.com/w/cpp/types/floating-point*
