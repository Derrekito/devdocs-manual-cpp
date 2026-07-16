# std::fixed, std::scientific, std::hexfloat, std::defaultfloat

These four manipulators set the stream's `floatfield` — the group
that controls *how* floating-point values are formatted. Exactly one
is active at a time; setting one clears the others.

```cpp skip
out << std::fixed;         // NNN.NNN, precision = digits after point
out << std::scientific;    // N.NNNe±NN, precision = digits after point
out << std::hexfloat;      // 0xH.HHHp±N, exact, ignores precision (C++11)
out << std::defaultfloat;  // shortest form, precision = sig digits (C++11)
```

### What you provide

- **str** — the stream (via `out << std::fixed`, etc.); works for any
  `std::basic_ostream` or `std::basic_istream`.

### Guarantees and costs

- `fixed` sets `floatfield` to `fixed`
  (`str.setf(ios_base::fixed, ios_base::floatfield)`).
- `scientific` sets it to `scientific`
  (`str.setf(ios_base::scientific, ios_base::floatfield)`).
- `hexfloat` sets both bits at once
  (`fixed | scientific`), producing hexadecimal floating-point.
- `defaultfloat` clears `floatfield`
  (`str.unsetf(ios_base::floatfield)`), restoring the default
  shortest-representation formatting.
- `setprecision` means different things under each: digits after the
  decimal point for `fixed`/`scientific`; **ignored entirely** for
  `hexfloat`; maximum significant digits for `defaultfloat`.
- None of the four affect floating-point *parsing* (`operator>>`) —
  only output formatting.

### Gotchas

- Switching floatfield without resetting precision can produce
  surprising digit counts — precision means something different in
  each mode.
- `hexfloat` output is exact but not portable to read by eye; use it
  for round-tripping bit-exact values, not for display.
- Only one floatfield is active at a time: setting `fixed` after
  `scientific` fully replaces it, it doesn't combine.

### Example

```cpp
#include <iomanip>
#include <iostream>

int main()
{
    double x{1.5};

    std::cout << std::defaultfloat << x << '\n'
              << std::fixed       << x << '\n'
              << std::scientific  << x << '\n'
              << std::hexfloat    << x << '\n';
}
```

```text
1.5
1.500000
1.500000e+00
0x1.8p+0
```

### Reference

```cpp skip
std::ios_base& fixed( std::ios_base& str );                     // (1)
std::ios_base& scientific( std::ios_base& str );                // (2)
std::ios_base& hexfloat( std::ios_base& str );      // (3) (since C++11)
std::ios_base& defaultfloat( std::ios_base& str );  // (4) (since C++11)
```

Each returns `str`. Hexadecimal floating-point formatting ignores the
stream's precision setting, per the specification of
`std::num_put::do_put`.

### See also

- **setprecision** — what precision means depends on which of these
  is active
- **setw** — field width for the next insertion only

---
*Source: https://en.cppreference.com/w/cpp/io/manip/fixed*
