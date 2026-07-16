# std::abs, std::labs, std::llabs, std::imaxabs

Absolute value for integers. The headline trap: on any 2's-complement
machine (i.e. virtually all of them), `abs(INT_MIN)` is **undefined
behavior** — the mathematical result doesn't fit back in the return
type. The second trap is header/overload confusion: `std::abs` is
overloaded across `<cstdlib>` for `int`/`long`/`long long` and across
`<cmath>` for floating-point — pick the wrong header or a mismatched
argument type and you may not get the overload you expect.

```cpp skip
std::abs(num);      // int/long/long long — <cstdlib>; also floating-point via <cmath>
std::labs(num);      // long              — <cstdlib>
std::llabs(num);      // long long          — <cstdlib>   (since C++11)
std::abs(intmax_t);   // std::intmax_t      — <cinttypes>  (since C++11)
std::imaxabs(num);    // std::intmax_t      — <cinttypes>  (since C++11)
```

`constexpr` since C++23.

### What you provide

- **num** — the integer value. If you call `std::abs` with an unsigned
  argument that integral promotion can't convert to `int`, the
  program is ill-formed (won't compile) rather than silently doing
  something wrong.

### Guarantees and costs

- Returns `|num|` when it is representable in the return type.
- Undefined behavior if the result cannot be represented — the classic
  case is `std::abs(INT_MIN)` on a 2's-complement `int`: for 32-bit
  `int`, `INT_MIN` is -2147483648 but the would-be result 2147483648
  exceeds `INT_MAX` (2147483647).
- The `std::intmax_t` overload of `std::abs` (6) is provided in
  `<cinttypes>` only when `std::intmax_t` is an extended integer type;
  otherwise it overlaps with one of the other integer overloads.
- Historically (fixed by LWG 2192), the integer overloads of
  `std::abs` were inconsistently declared across `<cstdlib>` and
  `<cmath>`; the fix requires both headers to declare all the integer
  overloads.

### Gotchas

- `std::abs(INT_MIN)` (or the `long`/`long long` equivalents at their
  respective minimums) is UB, not a defined "wraps to itself" or
  "saturates" result — don't rely on any particular observed value.
- Forgetting `<cstdlib>` (or `<cinttypes>` for the `intmax_t`
  overload) and having only `<cmath>` visible can silently select the
  floating-point `std::abs(double)` overload for an integer argument
  via implicit conversion, instead of failing to compile — include
  the header for the type you're actually passing.
- `std::abs` is unrelated to `std::abs` for `std::complex` or
  `std::valarray` (different headers, different meanings) — don't
  confuse the overload sets.

### Example

```cpp
#include <cstdlib>
#include <iostream>

int main()
{
    std::cout << std::showpos
              << "abs(+3) = " << std::abs(3) << '\n'
              << "abs(-3) = " << std::abs(-3) << '\n';

    // std::abs(INT_MIN); // undefined behavior on 2's complement systems
}
```

```text
abs(+3) = +3
abs(-3) = +3
```

### Reference

```cpp skip
int       abs( int num );  // (1) (constexpr since C++23)
long      abs( long num );  // (2) (constexpr since C++23)
long long abs( long long num );  // (3) (since C++11) (constexpr since C++23)
long       labs( long num );  // (4) (constexpr since C++23)
long long llabs( long long num );  // (5) (since C++11) (constexpr since C++23)
std::intmax_t abs( std::intmax_t num );  // (6) (since C++11) (constexpr since C++23)
std::intmax_t imaxabs( std::intmax_t num );  // (7) (since C++11) (constexpr since C++23)
```

### See also

- **abs** — absolute value of a floating-point value (`fabs`, `fabsf`,
  `fabsl`)
- **abs(std::complex)** — magnitude of a complex number
- **abs(std::valarray)** — applies `abs` to each element of a valarray

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/abs*
