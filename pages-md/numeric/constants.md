# Mathematical constants

### Constants (since C++20)

- **e_v** — the mathematical constant \(\small e\)e (variable template)
- **log2e_v** — \(\log_{2}e\)log2e (variable template)
- **log10e_v** — \(\log_{10}e\)log10e (variable template)
- **pi_v** — the mathematical constant \(\pi\)π (variable template)
- **inv_pi_v** — \(\frac1\pi\)1/π (variable template)
- **inv_sqrtpi_v** — \(\frac1{\sqrt\pi}\)1/√π (variable template)
- **ln2_v** — \(\ln{2}\)ln 2 (variable template)
- **ln10_v** — \(\ln{10}\)ln 10 (variable template)
- **sqrt2_v** — \(\sqrt2\)√2 (variable template)
- **sqrt3_v** — \(\sqrt3\)√3 (variable template)
- **inv_sqrt3_v** — \(\frac1{\sqrt3}\)1/√3 (variable template)
- **egamma_v** — the Euler–Mascheroni constant γ (variable template)
- **phi_v** — the golden ratio Φ (\(\frac{1+\sqrt5}2\)1 + √5/2) (variable
  template)
- **inline constexpr double e** — `e_v<double>` (constant)
- **inline constexpr double log2e** — `log2e_v<double>` (constant)
- **inline constexpr double log10e** — `log10e_v<double>` (constant)
- **inline constexpr double pi** — `pi_v<double>` (constant)
- **inline constexpr double inv_pi** — `inv_pi_v<double>` (constant)
- **inline constexpr double inv_sqrtpi** — `inv_sqrtpi_v<double>` (constant)
- **inline constexpr double ln2** — `ln2_v<double>` (constant)
- **inline constexpr double ln10** — `ln10_v<double>` (constant)
- **inline constexpr double sqrt2** — `sqrt2_v<double>` (constant)
- **inline constexpr double sqrt3** — `sqrt3_v<double>` (constant)
- **inline constexpr double inv_sqrt3** — `inv_sqrt3_v<double>` (constant)
- **inline constexpr double egamma** — `egamma_v<double>` (constant)
- **inline constexpr double phi** — `phi_v<double>` (constant)

### Notes

A program that instantiates a primary template of a mathematical constant
variable template is ill-formed.

The standard library specializes mathematical constant variable templates for
all floating-point types (i.e. `float`, `double` and `long double`).

A program may partially or explicitly specialize a mathematical constant
variable template provided that the specialization depends on a program-defined
type.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_math_constants` | 201907L | (c++20) | Mathematical constants

### Example

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>
#include <limits>
#include <numbers>
#include <string_view>

auto egamma_aprox(const unsigned iterations)
{
    long double s = 0;
    for (unsigned m = 2; m < iterations; ++m)
    {
        if (const long double t = std::riemann_zetal(m) / m; m % 2)
            s -= t;
        else
            s += t;
    }
    return s;
};

int main()
{
    using namespace std;
    using namespace std::numbers;

    const auto
        x = sqrt(inv_pi)/inv_sqrtpi + ceil(exp2(log2e)) + sqrt3*inv_sqrt3 + exp(0),
        v = (phi*phi - phi) + 1/log2(sqrt2) + log10e*ln10 + pow(e, ln2) - cos(pi);

    std::cout << "The answer is " << x*v << '\n';

    using namespace std::string_view_literals;
    constexpr auto γ = "0.577215664901532860606512090082402"sv;

    std::cout
        << "γ as 10⁶ sums of ±ζ(m)/m   = "
        << egamma_aprox(1'000'000) << '\n'
        << "γ as egamma_v<float>       = "
        << std::setprecision(std::numeric_limits<float>::digits10 + 1)
        << egamma_v<float> << '\n'
        << "γ as egamma_v<double>      = "
        << std::setprecision(std::numeric_limits<double>::digits10 + 1)
        << egamma_v<double> << '\n'
        << "γ as egamma_v<long double> = "
        << std::setprecision(std::numeric_limits<long double>::digits10 + 1)
        << egamma_v<long double> << '\n'
        << "γ with " << γ.length() - 1 << " digits precision = " << γ << '\n'
    ;
}
```

Possible output:

```text
The answer is 42
γ as 10⁶ sums of ±ζ(m)/m   = 0.577215
γ as egamma_v<float>       = 0.5772157
γ as egamma_v<double>      = 0.5772156649015329
γ as egamma_v<long double> = 0.5772156649015328606
γ with 34 digits precision = 0.577215664901532860606512090082402
```

### See also

- **ratio (C++11)** — represents exact rational fraction (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/constants*
