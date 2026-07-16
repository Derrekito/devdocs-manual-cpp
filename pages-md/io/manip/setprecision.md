# std::setprecision

Sets the stream's floating-point precision — and, unlike `setw`, it
**sticks** until changed again. What "precision" *means* depends on
the floatfield: with the default (or `std::defaultfloat`), it's the
maximum number of **significant digits** shown; with `std::fixed` (or
`std::scientific`), it's the number of **digits after the decimal
point**. Same call, different meaning — set `fixed` first if you want
digit-after-the-point control.

```cpp skip
out << std::setprecision(n) << value;   // sticky: applies to all later output
```

### What you provide

- **n** — new precision, as an `int`.

### Guarantees and costs

- Equivalent to calling `str.precision(n)` on the stream.
- Persists across insertions until a later `setprecision` call — no
  automatic reset, unlike `setw`.
- Default stream precision is 6.
- Usable with any character stream, not just
  `std::ostream`/`std::istream` (LWG 183).

### Gotchas

- Without `fixed`/`scientific`, `setprecision(n)` caps *significant*
  digits, so small and large magnitudes truncate differently — it is
  not "n digits after the point" until `fixed` is also set.
- It's sticky: a `setprecision` set for one value keeps affecting
  every later `<<`, including ones far away in the code, until reset.
- `std::hexfloat` ignores the precision setting entirely (see
  `std::fixed`).

### Example

```cpp
#include <iomanip>
#include <iostream>

int main()
{
    constexpr double pi{3.14159265358979};

    std::cout << "default (sig figs):    " << pi << '\n'
              << std::setprecision(3)
              << "setprecision(3):       " << pi << '\n'
              << std::fixed
              << "fixed + precision(3):  " << pi << '\n';
}
```

```text
default (sig figs):    3.14159
setprecision(3):       3.14
fixed + precision(3):  3.142
```

### Reference

```cpp skip
/* unspecified */ setprecision( int n );
```

Returns an object of unspecified type such that, when written to an
output stream or read from an input stream, it calls `str.precision(n)`
on the stream and otherwise behaves like the stream itself.

### See also

- **fixed**, **scientific**, **hexfloat**, **defaultfloat** — choose
  what precision measures
- **setw** — field width for the next insertion only (not sticky)
- **precision** — the `ios_base` member `setprecision` calls

---
*Source: https://en.cppreference.com/w/cpp/io/manip/setprecision*
