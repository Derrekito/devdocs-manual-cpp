# std::tanh(std::valarray)

```cpp
template< class T >
valarray<T> tanh( const valarray<T>& va );
```

For each element in `va` computes hyperbolic tangent of the value of the
element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing hyperbolic tangent of the values in `va`.

### Notes

Unqualified function (`tanh`) is used to perform the computation. If such
function is not available, `std::tanh` is used due to argument-dependent lookup.

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
valarray<T> tanh(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = tanh(i);

    return other; // proxy object may be returned
}
```

### Example

```cpp
#include <cmath>
#include <iostream>
#include <valarray>

auto show = [](char const* title, const std::valarray<double>& va)
{
    std::cout << title << " :";
    for (auto x : va)
        std::cout << "  " << std::fixed << x;
    std::cout << '\n';
};

int main()
{
    const std::valarray<double> x = {.0, .1, .2, .3};
    const std::valarray<double> sinh = std::sinh(x);
    const std::valarray<double> cosh = std::cosh(x);
    const std::valarray<double> tanh = std::tanh(x);
    const std::valarray<double> tanh_by_def = sinh / cosh;
    const std::valarray<double> tanh_2x = std::tanh(2.0 * x);
    const std::valarray<double> tanh_2x_by_def =
        (2.0 * tanh) / (1.0 + std::pow(tanh, 2.0));

    show("x              ", x);
    show("tanh(x)        ", tanh);
    show("tanh(x) (def)  ", tanh_by_def);
    show("tanh(2*x)      ", tanh_2x);
    show("tanh(2*x) (def)", tanh_2x_by_def);
}
```

Output:

```text
x               :  0.000000  0.100000  0.200000  0.300000
tanh(x)         :  0.000000  0.099668  0.197375  0.291313
tanh(x) (def)   :  0.000000  0.099668  0.197375  0.291313
tanh(2*x)       :  0.000000  0.197375  0.379949  0.537050
tanh(2*x) (def) :  0.000000  0.197375  0.379949  0.537050
```

### See also

- **sinh(std::valarray)** — applies the function `std::sinh` to each element of
  valarray (function template)
- **cosh(std::valarray)** — applies the function `std::cosh` to each element of
  valarray (function template)
- **tanhtanhftanhl (C++11)(C++11)** — computes hyperbolic tangent
  (\({\small\tanh{x}}\)tanh(x)) (function)
- **tanh(std::complex)** — computes hyperbolic tangent of a complex number
  (\({\small\tanh{z}}\)tanh(z)) (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/tanh*
