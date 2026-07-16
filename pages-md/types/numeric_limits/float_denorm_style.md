# std::float_denorm_style

```cpp
enum float_denorm_style {
    denorm_indeterminate = -1,
    denorm_absent        = 0,
    denorm_present       = 1
};  // (deprecated in C++23)
```

Enumeration constants of type `std::float_denorm_style` indicate support of
subnormal values by floating-point types.

### Enumeration constants

- **`std::denorm_indeterminate`** — Support of subnormal values cannot be
  determined
- **`std::denorm_absent`** — The type does not support subnormal values
- **`std::denorm_present`** — The type allows subnormal values

### See also

- **has_denorm [static]** — identifies the denormalization style used by the
  floating-point type (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/float_denorm_style*
