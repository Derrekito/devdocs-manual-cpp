# std::sinh(std::valarray)

```cpp
template< class T >
valarray<T> sinh( const valarray<T>& va );
```

For each element in `va` computes hyperbolic sine of the value of the element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing hyperbolic sine of the values in `va`.

### Notes

Unqualified function (`sinh`) is used to perform the computation. If such
function is not available, `std::sinh` is used due to argument-dependent lookup.

The function can be implemented with the return type different from
`std::valarray`. In this case, the replacement type has the following
properties:

- All const member functions of `std::valarray` are provided.
- `std::valarray`, `std::slice_array`, `std::gslice_array`, `std::mask_array`
  and `std::indirect_array` can be constructed from the replacement type.
- For every function taking a const std::valarray<T>& except `begin()` and
  `end()`(since C++11), identical functions taking the replacement types shall
  be added;
- For every function taking two const std::valarray<T>& arguments, identical
  functions taking every combination of const std::valarray<T>& and replacement
  types shall be added.
- The return type does not add more than two levels of template nesting over the
  most deeply-nested argument type.

### Possible implementation

```cpp
template<class T>
valarray<T> sinh(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = sinh(i);

    return other; // proxy object may be returned
}
```

### Example

```cpp
#include <cmath>
#include <complex>
#include <iomanip>
#include <iostream>
#include <valarray>

template<typename T>
void show(char const* title, const std::valarray<T>& va)
{
    std::cout << title << " : " << std::right;
    for (T x : va)
        std::cout << std::fixed << x << ' ';
    std::cout << '\n';
}

template<typename T>
void sinh_for(std::valarray<T> const& z)
{
    // Hyperbolic sine is sinh(z) = (eᶻ - e⁻ᶻ) / 2.

    const std::valarray<T> sinh_z{std::sinh(z)};
    const std::valarray<T> e_z{std::exp(z)};
    const std::valarray<T> e_neg_z{std::exp(-z)};
    const std::valarray<T> sinh_def{(e_z - e_neg_z) / 2.0f};

    show("n         ", z);
    show("sinh(n)   ", sinh_z);
    show("(eⁿ-e⁻ⁿ)/2", sinh_def);

    std::cout.put('\n');
}

int main()
{
    sinh_for(std::valarray<float>{-.2f, -.1f, 0.f, .1f, .2f, INFINITY});
    sinh_for(std::valarray<std::complex<double>>{{-.2,-.1}, {.2,.1}});
}
```

Output:

```text
n          : -0.200000 -0.100000 0.000000 0.100000 0.200000 inf
sinh(n)    : -0.201336 -0.100167 0.000000 0.100167 0.201336 inf
(eⁿ-e⁻ⁿ)/2 : -0.201336 -0.100167 0.000000 0.100167 0.201336 inf

n          : (-0.200000,-0.100000) (0.200000,0.100000)
sinh(n)    : (-0.200330,-0.101837) (0.200330,0.101837)
(eⁿ-e⁻ⁿ)/2 : (-0.200330,-0.101837) (0.200330,0.101837)
```

### See also

- **cosh(std::valarray)** — applies the function `std::cosh` to each element of
  valarray (function template)
- **tanh(std::valarray)** — applies the function `std::tanh` to each element of
  valarray (function template)
- **sinhsinhfsinhl (C++11)(C++11)** — computes hyperbolic sine
  (\({\small\sinh{x}}\)sinh(x)) (function)
- **sinh(std::complex)** — computes hyperbolic sine of a complex number
  (\({\small\sinh{z}}\)sinh(z)) (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/sinh*
