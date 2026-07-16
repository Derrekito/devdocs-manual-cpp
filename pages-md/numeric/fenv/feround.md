# std::fegetround, std::fesetround

```cpp
int fesetround( int round )  // (1) (since C++11)
int fegetround()  // (2) (since C++11)
```

Manages the floating-point rounding direction.

1) Attempts to establish the floating-point rounding direction equal to the
   argument `round`, which is expected to be one of the floating point rounding
   macros.

2) Returns the value of the floating point rounding macro that corresponds to
   the current rounding direction.

### Parameters

- **round** — rounding direction, one of floating point rounding macros

### Return value

1) `​0​` on success, non-zero otherwise.

2) The floating point rounding macro describing the current rounding direction
or a negative value if the direction cannot be determined.

### Notes

The current rounding mode, reflecting the effects of the most recent
`fesetround`, can also be queried with `FLT_ROUNDS`.

See floating-point rounding macros for the effects of rounding.

### Example

```cpp
#include <cfenv>
#include <cmath>
#include <iomanip>
#include <iostream>
#include <utility>
// #pragma STDC FENV_ACCESS ON

int main()
{
    static constexpr std::pair<const char*, const double> samples[]
    {
        {" 12.0", 12.0},  {" 12.1", 12.1}, {"-12.1", -12.1}, {" 12.5", 12.5},
        {"-12.5", -12.5}, {" 12.9", 12.9}, {"-12.9", -12.9}, {" 13.0", 13.0}
    };

    std::cout <<
        "│ sample │  FE_DOWNWARD  │   FE_UPWARD   │ FE_TONEAREST  │ FE_TOWARDZERO │\n";

    for (const auto& [str, fp] : samples)
    {
        std::cout << "│ " << std::setw(6) << str << " │  ";
        for (const int dir : {FE_DOWNWARD, FE_UPWARD, FE_TONEAREST, FE_TOWARDZERO})
        {
            std::fesetround(dir);
            std::cout << std::setw(10) << std::fixed << std::nearbyint(fp) << "   │  ";
        }
        std::cout << '\n';
    }
}
```

Output:

```text
│ sample │  FE_DOWNWARD  │   FE_UPWARD   │ FE_TONEAREST  │ FE_TOWARDZERO │
│   12.0 │   12.000000   │   12.000000   │   12.000000   │   12.000000   │
│   12.1 │   12.000000   │   13.000000   │   12.000000   │   12.000000   │
│  -12.1 │  -13.000000   │  -12.000000   │  -12.000000   │  -12.000000   │
│   12.5 │   12.000000   │   13.000000   │   12.000000   │   12.000000   │
│  -12.5 │  -13.000000   │  -12.000000   │  -12.000000   │  -12.000000   │
│   12.9 │   12.000000   │   13.000000   │   13.000000   │   12.000000   │
│  -12.9 │  -13.000000   │  -12.000000   │  -13.000000   │  -12.000000   │
│   13.0 │   13.000000   │   13.000000   │   13.000000   │   13.000000   │
```

### See also

- **nearbyintnearbyintfnearbyintl (C++11)(C++11)(C++11)** — nearest integer
  using current rounding mode (function)
- **rintrintfrintllrintlrintflrintlllrintllrintfllrintl
  (C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)(C++11)** — nearest
  integer using current rounding mode with exception if the result differs
  (function)

**C documentation for `fegetround, fesetround`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/feround*
