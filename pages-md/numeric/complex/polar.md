# std::polar(std::complex)

```cpp
template< class T >
std::complex<T> polar( const T& r, const T& theta = T() );
```

Returns a complex number with magnitude `r` and phase angle `theta`.

The behavior is undefined if `r` is negative or NaN, or if `theta` is infinite.

### Parameters

- **r** — magnitude
- **theta** — phase angle

### Return value

A complex number determined by `r` and `theta`.

### Notes

`std::polar(r, theta)` is equivalent to any of the following expressions:

- `r * std::exp(theta * 1i)`
- `r * (cos(theta) + sin(theta) * 1i)`
- `std::complex(r * cos(theta), r * sin(theta))`.

Using polar instead of exp can be about **4.5x** faster in vectorized loops.

### Example

```cpp
#include <cmath>
#include <complex>
#include <iomanip>
#include <iostream>
#include <numbers>
using namespace std::complex_literals;

int main()
{
    constexpr auto π_2{std::numbers::pi / 2.0};
    constexpr auto mag{1.0};

    std::cout
        << std::fixed << std::showpos << std::setprecision(1)
        << "   θ: │ polar:      │ exp:        │ complex:    │ trig:\n";
    for (int n{}; n != 4; ++n)
    {
        const auto θ{n * π_2};
        std::cout << std::setw(4) << 90 * n << "° │ "
                  << std::polar(mag, θ) << " │ "
                  << mag * std::exp(θ * 1.0i) << " │ "
                  << std::complex(mag * cos(θ), mag * sin(θ)) << " │ "
                  << mag * (cos(θ) + 1.0i * sin(θ)) << '\n';
    }
}
```

Output:

```text
   θ: │ polar:      │ exp:        │ complex:    │ trig:
  +0° │ (+1.0,+0.0) │ (+1.0,+0.0) │ (+1.0,+0.0) │ (+1.0,+0.0)
 +90° │ (+0.0,+1.0) │ (+0.0,+1.0) │ (+0.0,+1.0) │ (+0.0,+1.0)
+180° │ (-1.0,+0.0) │ (-1.0,+0.0) │ (-1.0,+0.0) │ (-1.0,+0.0)
+270° │ (-0.0,-1.0) │ (-0.0,-1.0) │ (-0.0,-1.0) │ (-0.0,-1.0)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2459 | C++98 | behavior unclear for some inputs | made undefined
  LWG 2870 | C++98 | default value of parameter `theta` not dependent | made
      dependent

### See also

- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)
- **arg** — returns the phase angle (function template)
- **exp(std::complex)** — complex base *e* exponential (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/polar*
