# std::clamp

Confines a value to the closed range `[lo, hi]`: below `lo` returns
`lo`, above `hi` returns `hi`, otherwise returns the value unchanged.
Available since C++17.

```cpp skip
std::clamp(v, lo, hi);        // clamp by operator<   (since C++17)
std::clamp(v, lo, hi, comp);  // clamp by comp         (since C++17)
```

Both overloads are `constexpr`.

### What you provide

- **v** — the value to clamp.
- **lo, hi** — the inclusive boundaries; `lo` must not compare greater
  than `hi`.
- **comp** — a callable answering *is the first argument less than the
  second?*, returning `bool`. Must not modify its arguments. Defaults
  to `operator<`.

### Guarantees and costs

- At most two comparisons.
- Returns a reference: to `lo` if `v` is less than `lo`, to `hi` if
  `hi` is less than `v`, otherwise to `v` itself. If `v` compares
  equivalent to a bound, the reference returned is to `v`, not the
  bound.
- Undefined behavior if `lo` compares greater than `hi`.

### Gotchas

- `hi < lo` is undefined behavior with no diagnostic — validate bounds
  that come from input or configuration.
- All three arguments must be the same type: `std::clamp(x, 0, 255)`
  with a `double x` fails to deduce; use matching literals (`0.0`,
  `255.0`) or `std::clamp<double>(...)`.
- It returns a **reference** to one of its arguments; assigning it
  while an argument is a temporary can dangle. The common form
  `x = std::clamp(x, lo, hi)` with named bounds is safe.
- `v`, `lo`, or `hi` being NaN makes the result unspecified — every
  comparison against NaN is `false`, so don't assume NaN gets clamped
  to a bound or passed through untouched.

### Example

```cpp
#include <algorithm>
#include <iostream>

int main()
{
    std::cout << std::clamp(5, 0, 10)  << ' '
              << std::clamp(-3, 0, 10) << ' '
              << std::clamp(42, 0, 10) << '\n';
}
```

```text
5 0 10
```

### Reference

Full declarations:

```cpp skip
template< class T >
constexpr const T& clamp( const T& v,
                          const T& lo, const T& hi );  // (1) (since C++17)
template< class T, class Compare >
constexpr const T& clamp( const T& v, const T& lo, const T& hi,
                          Compare comp );  // (2) (since C++17)
```

`T` must be LessThanComparable for overload (1); a floating-point `T`
qualifies as long as NaN is avoided. Feature-test macro:
`__cpp_lib_clamp` (`201603L`, C++17).

### See also

- **min** — returns the smaller of the given values
- **max** — returns the greater of the given values
- **in_range (C++20)** — checks if a value fits in a given integer type
- **ranges::clamp (C++20)** — constrained version of this function

---
*Source: https://en.cppreference.com/w/cpp/algorithm/clamp*
