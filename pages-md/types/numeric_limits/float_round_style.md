# std::float_round_style

```cpp
enum float_round_style {
    round_indeterminate       = -1,
    round_toward_zero         = 0,
    round_to_nearest          = 1,
    round_toward_infinity     = 2,
    round_toward_neg_infinity = 3
};
```

Enumeration constants of type `std::float_round_style` indicate the rounding
style used by floating-point arithmetic whenever a result of an expression is
stored in an object of a floating-point type. The values are:

### Enumeration constants

- **`std::round_indeterminate`** — Rounding style cannot be determined
- **`std::round_toward_zero`** — Rounding toward zero
- **`std::round_to_nearest`** — Rounding toward nearest representable value
- **`std::round_toward_infinity`** — Rounding toward positive infinity
- **`std::round_toward_neg_infinity`** — Rounding toward negative infinity

### See also

- **round_style [static]** — identifies the rounding style used by the type
  (public static member constant)
- **FE_DOWNWARDFE_TONEARESTFE_TOWARDZEROFE_UPWARD (C++11)** — floating-point
  rounding direction (macro constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/float_round_style*
