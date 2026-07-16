# std::numeric_limits<T>::traps

```cpp
static const bool traps;  // (until C++11)
static constexpr bool traps;  // (since C++11)
```

The value of `std::numeric_limits<T>::traps` is `true` for all arithmetic types
`T` that have at least one value at the start of the program that, if used as an
argument to an arithmetic operation, will generate a trap.

### Standard specializations

- **/* non-specialized */** — `false`
- **bool** — `false`
- **char** — usually `true`
- **signed char** — usually `true`
- **unsigned char** — usually `true`
- **wchar_t** — usually `true`
- **char8_t (since C++20)** — usually `true`
- **char16_t (since C++11)** — usually `true`
- **char32_t (since C++11)** — usually `true`
- **short** — usually `true`
- **unsigned short** — usually `true`
- **int** — usually `true`
- **unsigned int** — usually `true`
- **long** — usually `true`
- **unsigned long** — usually `true`
- **long long (since C++11)** — usually `true`
- **unsigned long long (since C++11)** — usually `true`
- **float** — usually `false`
- **double** — usually `false`
- **long double** — usually `false`

### Notes

On most platforms integer division by zero always traps, and
`std::numeric_limits<T>::traps` is `true` for all integer types that support the
value `​0​`. The exception is the type bool: even though division by `false`
traps due to integral promotion from bool to int, it is the zero-valued int that
traps. Zero is not a value of type bool.

On most platforms, floating-point exceptions may be turned on and off at run
time (e.g. `feenableexcept()` on Linux or `_controlfp` on Windows), in which
case the value of `std::numeric_limits<T>::traps` for floating-point types
reflects the state of floating-point trapping facility at the time of program
startup, which is `false` on most modern systems. An exception would be a DEC
Alpha program, where it is `true` if compiled without `-ieee`.

### Example

```cpp
#include <iostream>
#include <limits>

int main()
{
    std::cout << std::boolalpha
              << "bool:     traps = " << std::numeric_limits<bool>::traps << '\n'
              << "char:     traps = " << std::numeric_limits<char>::traps << '\n'
              << "char16_t: traps = " << std::numeric_limits<char16_t>::traps << '\n'
              << "long:     traps = " << std::numeric_limits<long>::traps << '\n'
              << "float:    traps = " << std::numeric_limits<float>::traps << '\n';
}
```

Possible output:

```text
// GCC output:
bool:     traps = true
char:     traps = true
char16_t: traps = true
long:     traps = true
float:    traps = false

// Clang output:
bool:     traps = false
char:     traps = true
char16_t: traps = true
long:     traps = true
float:    traps = false
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 497 | C++98 | it was unclear what is returned if trapping is enabled or
      disabled at runtime | returns the enable status at the start of the
      program

### See also

**Floating-point environment**

- **tinyness_before [static]** — identifies floating-point types that detect
  tinyness before rounding (public static member constant)
- **has_denorm_loss [static]** — identifies the floating-point types that detect
  loss of precision as denormalization loss rather than inexact result (public
  static member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/traps*
