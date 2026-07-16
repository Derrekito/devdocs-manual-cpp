# std::expint, std::expintf, std::expintl

```cpp
float       expint ( float num );
double      expint ( double num );
long double expint ( long double num );  // (since C++17) (until C++23)
/* floating-point-type */ expint( /* floating-point-type */ num );  // (since C++23)
float       expintf( float num );  // (2) (since C++17)
long double expintl( long double num );  // (3) (since C++17)
Additional overloads
template< class Integer >
double      expint ( Integer num );  // (A) (since C++17)
```

1-3) Computes the Exponential integral of `num`. The library provides overloads
   of `std::expint` for all cv-unqualified floating-point types as the type of
   the parameter `num`.(since C++23)

A) Additional overloads are provided for all integer types, which are treated as
   `double`.

### Parameters

- **num** — floating-point or integer value

### Return value

If no errors occur, value of the exponential integral of `num`, that is
-∫∞-nume-t/td*t*, is returned.

### Error handling

Errors may be reported as specified in `math_errhandling`.

- If the argument is NaN, NaN is returned and domain error is not reported.
- If the argument is ±0, -∞ is returned.

### Notes

Implementations that do not support C++17, but support ISO 29124:2010, provide
this function if `__STDCPP_MATH_SPEC_FUNCS__` is defined by the implementation
to a value at least 201003L and if the user defines
`__STDCPP_WANT_MATH_SPEC_FUNCS__` before including any standard library headers.

Implementations that do not support ISO 29124:2010 but support TR 19768:2007
(TR1), provide this function in the header `tr1/cmath` and namespace `std::tr1`.

An implementation of this function is also available in boost.math.

The additional overloads are not required to be provided exactly as (A). They
only need to be sufficient to ensure that for their argument `num` of integer
type, `std::expint(num)` has the same effect as
`std::expint(static_cast<double>(num))`.

### Example

```cpp
#include <algorithm>
#include <cmath>
#include <iostream>
#include <vector>

template<int Height = 5, int BarWidth = 1, int Padding = 1, int Offset = 0, class Seq>
void draw_vbars(Seq&& s, const bool DrawMinMax = true)
{
    static_assert(0 < Height and 0 < BarWidth and 0 <= Padding and 0 <= Offset);

    auto cout_n = [](auto&& v, int n = 1)
    {
        while (n-- > 0)
            std::cout << v;
    };

    const auto [min, max] = std::minmax_element(std::cbegin(s), std::cend(s));

    std::vector<std::div_t> qr;
    for (typedef decltype(*std::cbegin(s)) V; V e : s)
        qr.push_back(std::div(std::lerp(V(0), 8 * Height,
                                        (e - *min) / (*max - *min)), 8));

    for (auto h{Height}; h-- > 0; cout_n('\n'))
    {
        cout_n(' ', Offset);

        for (auto dv : qr)
        {
            const auto q{dv.quot}, r{dv.rem};
            unsigned char d[]{0xe2, 0x96, 0x88, 0}; // Full Block: '█'
            q < h ? d[0] = ' ', d[1] = 0 : q == h ? d[2] -= (7 - r) : 0;
            cout_n(d, BarWidth), cout_n(' ', Padding);
        }

        if (DrawMinMax && Height > 1)
            Height - 1 == h ? std::cout << "┬ " << *max:
                          h ? std::cout << "│ "
                            : std::cout << "┴ " << *min;
    }
}

int main()
{
    std::cout << "Ei(0) = " << std::expint(0) << '\n'
              << "Ei(1) = " << std::expint(1) << '\n'
              << "Gompertz constant = " << -std::exp(1) * std::expint(-1) << '\n';

    std::vector<float> v;
    for (float x{1.f}; x < 8.8f; x += 0.3565f)
        v.push_back(std::expint(x));
    draw_vbars<9, 1, 1>(v);
}
```

Output:

```text
Ei(0) = -inf
Ei(1) = 1.89512
Gompertz constant = 0.596347
                                          █ ┬ 666.505
                                          █ │
                                        ▆ █ │
                                        █ █ │
                                      █ █ █ │
                                    ▆ █ █ █ │
                                ▁ ▆ █ █ █ █ │
                            ▂ ▅ █ █ █ █ █ █ │
▁ ▁ ▁ ▁ ▁ ▁ ▁ ▂ ▂ ▃ ▃ ▄ ▆ ▇ █ █ █ █ █ █ █ █ ┴ 1.89512
```

### External links

  Weisstein, Eric W. "Exponential Integral." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions/expint*
