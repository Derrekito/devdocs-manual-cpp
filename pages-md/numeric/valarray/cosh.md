# std::cosh(std::valarray)

```cpp
template< class T >
valarray<T> cosh( const valarray<T>& va );
```

For each element in `va` computes hyperbolic cosine of the value of the element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing hyperbolic cosine of the values in `va`.

### Notes

Unqualified function (`cosh`) is used to perform the computation. If such
function is not available, `std::cosh` is used due to argument-dependent lookup.

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
valarray<T> cosh(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = cosh(i);

    return other; // proxy object may be returned
}
```

### Example

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>
#include <valarray>

void show(const char* title, const std::valarray<float>& data)
{
    const int w{9};
    std::cout << std::setw(w) << title << " | ";
    for (float x : data)
        std::cout << std::setw(w) << x << " | ";
    std::cout << '\n';
}

int main()
{
    const std::valarray<float> x{.1, .2, .3, .4};
    const auto sinh = std::sinh(x);
    const auto cosh = std::cosh(x);
    const auto z = (cosh * cosh) - (sinh * sinh);

    show("x", x);
    show("sinh(x)", sinh);
    show("cosh(x)", cosh);
    show("z", z);
}
```

Output:

```text
      x |       0.1 |       0.2 |       0.3 |       0.4 |
sinh(x) |  0.100167 |  0.201336 |   0.30452 |  0.410752 |
cosh(x) |     1.005 |   1.02007 |   1.04534 |   1.08107 |
      z |         1 |         1 |         1 |         1 |
```

### See also

- **sinh(std::valarray)** — applies the function `std::sinh` to each element of
  valarray (function template)
- **tanh(std::valarray)** — applies the function `std::tanh` to each element of
  valarray (function template)
- **coshcoshfcoshl (C++11)(C++11)** — computes hyperbolic cosine
  (\({\small\cosh{x}}\)cosh(x)) (function)
- **cosh(std::complex)** — computes hyperbolic cosine of a complex number
  (\({\small\cosh{z}}\)cosh(z)) (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/cosh*
