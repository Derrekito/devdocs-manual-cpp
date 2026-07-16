# std::erf, std::erff, std::erfl

```cpp
float       erf ( float num );
double      erf ( double num );
long double erf ( long double num );  // (until C++23)
/* floating-point-type */
            erf ( /* floating-point-type */ num );  // (since C++23) (constexpr since C++26)
float       erff( float num );  // (2) (since C++11) (constexpr since C++26)
long double erfl( long double num );  // (3) (since C++11) (constexpr since C++26)
Additional overloads (since C++11)
template< class Integer >
double      erf ( Integer num );  // (A) (constexpr since C++26)
```

1-3) Computes the error function of `num`. The library provides overloads of
   `std::erf` for all cv-unqualified floating-point types as the type of the
   parameter.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
double.
*(since C++11)*

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, value of the error function of `num`, that is
\(\frac{2}{\sqrt{\pi} }\int_{0}^{num}{e^{-{t^2}
}\mathsf{d}t}\)2/√π∫num0*e*-t2d*t*, is returned.
If a range error occurs due to underflow, the correct result (after rounding),
that is \(\frac{2\cdot num}{\sqrt{\pi} }\)2*num/√π is returned.

### Error handling

Errors are reported as specified in `math_errhandling`.

If the implementation supports IEEE floating-point arithmetic (IEC 60559),

- If the argument is ±0, ±0 is returned.
- If the argument is ±∞, ±1 is returned.
- If the argument is NaN, NaN is returned.

### Notes

Underflow is guaranteed if `|num| < DBL_MIN * (std::sqrt(π) / 2)`.
\(\operatorname{erf}(\frac{x}{\sigma \sqrt{2} })\)erf(

x/σ√2) is the probability that a measurement whose errors are subject to a
normal distribution with standard deviation \(\sigma\)σ is less than \(x\)x away
from the mean value.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::erf(num)` has the same effect as
`std::erf(static_cast<double>(num))`.

### Example

The following example calculates the probability that a normal variate is on the
interval (x1, x2):

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>

double phi(double x1, double x2)
{
    return (std::erf(x2 / std::sqrt(2)) - std::erf(x1 / std::sqrt(2))) / 2;
}

int main()
{
    std::cout << "Normal variate probabilities:\n"
              << std::fixed << std::setprecision(2);
    for (int n = -4; n < 4; ++n)
        std::cout << '[' << std::setw(2) << n
                  << ':' << std::setw(2) << n + 1 << "]: "
                  << std::setw(5) << 100 * phi(n, n + 1) << "%\n";

    std::cout << "Special values:\n"
              << "erf(-0) = " << std::erf(-0.0) << '\n'
              << "erf(Inf) = " << std::erf(INFINITY) << '\n';
}
```

Output:

```text
Normal variate probabilities:
[-4:-3]:  0.13%
[-3:-2]:  2.14%
[-2:-1]: 13.59%
[-1: 0]: 34.13%
[ 0: 1]: 34.13%
[ 1: 2]: 13.59%
[ 2: 3]:  2.14%
[ 3: 4]:  0.13%
Special values:
erf(-0) = -0.00
erf(Inf) = 1.00
```

### See also

- **erfcerfcferfcl (C++11)(C++11)(C++11)** — complementary error function
  (function)

**C documentation for `erf`**

### External links

  Weisstein, Eric W. "Erf." From MathWorld — A Wolfram Web Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/erf*
