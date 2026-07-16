# std::exp(std::valarray)

```cpp
template< class T >
valarray<T> exp( const valarray<T>& va );
```

For each element in `va` computes *e* raised to the power equal to the value of
the element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing *e* raised by the values in `va`.

### Notes

Unqualified function (`exp`) is used to perform the computation. If such
function is not available, `std::exp` is used due to argument-dependent lookup.

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
valarray<T> exp(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = exp(i);

    return other; // proxy object may be returned
}
```

### Example

This example demonstrates the Euler's identity eiπ = -1 and the related
exponents.

```cpp
#include <complex>
#include <iostream>
#include <numbers>
#include <valarray>

int main()
{
    const double pi = std::numbers::pi;
    std::valarray<std::complex<double>> v =
    {
        {0, 0}, {0, pi / 2}, {0, pi}, {0, 3 * pi / 2}, {0, 2 * pi}
    };
    std::valarray<std::complex<double>> v2 = std::exp(v);
    for (std::cout << std::showpos << std::fixed; auto n : v2)
        std::cout << n << '\n';
}
```

Output:

```text
(+1.000000,+0.000000)
(+0.000000,+1.000000)
(-1.000000,+0.000000)
(-0.000000,-1.000000)
(+1.000000,-0.000000)
```

### See also

- **log(std::valarray)** — applies the function `std::log` to each element of
  valarray (function template)
- **expexpfexpl (C++11)(C++11)** — returns *e* raised to the given power
  (\({\small e^x}\)ex) (function)
- **exp(std::complex)** — complex base *e* exponential (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/exp*
