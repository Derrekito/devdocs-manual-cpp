# FLT_ROUNDS

```cpp
#define FLT_ROUNDS /* implementation defined */
```

Specifies the current rounding direction of floating-point arithmetic
operations.

- **`-1`** — the default rounding direction is not known
- **`0`** — toward zero; same meaning as `FE_TOWARDZERO`
- **`1`** — to nearest; same meaning as `FE_TONEAREST`
- **`2`** — towards positive infinity; same meaning as `FE_UPWARD`
- **`3`** — towards negative infinity; same meaning as `FE_DOWNWARD`
- **other values** — implementation-defined behavior

### Notes

The rounding mode can be changed with `std::fesetround` and `FLT_ROUNDS`
reflects that change.

The possible values of FLT_ROUNDS match the possible values of
`std::float_round_style`, returned by `std::numeric_limits::round_style`.

### See also

- **float_round_style** — indicates floating-point rounding modes (enum)
- **fegetroundfesetround (C++11)(C++11)** — gets or sets rounding direction
  (function)
- **FE_DOWNWARDFE_TONEARESTFE_TOWARDZEROFE_UPWARD (C++11)** — floating-point
  rounding direction (macro constant)

**C documentation for `FLT_ROUNDS`**

---
*Source: https://en.cppreference.com/w/cpp/types/climits/FLT_ROUNDS*
