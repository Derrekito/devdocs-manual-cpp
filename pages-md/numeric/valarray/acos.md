# std::acos(std::valarray)

```cpp
template< class T >
valarray<T> acos( const valarray<T>& va );
```

For each element in `va` computes arc cosine of the value of the element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing arc cosines of the values in `va`.

### Notes

Unqualified function (`acos`) is used to perform the computation. If such
function is not available, `std::acos` is used due to argument-dependent lookup.

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
valarray<T> acos(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = acos(i);

    return other; // proxy object may be returned
}
```

### Example

```cpp
#include <cmath>
#include <iostream>
#include <numbers>
#include <valarray>

int main()
{
    // take common x-values from unit circle
    const double s32 = std::sqrt(3.0) / 2.0;
    const double s22 = std::sqrt(2.0) / 2.0;
    std::valarray<double> v1 = {-1.0, -s32, -s22, -0.5, 0.0, 0.5, s22, s32, 1.0};
    std::valarray<double> v2 = std::acos(v1) * 180.0 / std::numbers::pi;

    for (double n : v2)
        std::cout << n << "° ";
    std::cout << '\n';
}
```

Output:

```text
180° 150° 135° 120° 90° 60° 45° 30° 0°
```

### See also

- **asin(std::valarray)** — applies the function `std::asin` to each element of
  valarray (function template)
- **atan(std::valarray)** — applies the function `std::atan` to each element of
  valarray (function template)
- **atan2(std::valarray)** — applies the function `std::atan2` to a valarray and
  a value (function template)
- **cos(std::valarray)** — applies the function `std::cos` to each element of
  valarray (function template)
- **acosacosfacosl (C++11)(C++11)** — computes arc cosine
  (\({\small\arccos{x}}\)arccos(x)) (function)
- **acos(std::complex) (C++11)** — computes arc cosine of a complex number
  (\({\small\arccos{z}}\)arccos(z)) (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/acos*
