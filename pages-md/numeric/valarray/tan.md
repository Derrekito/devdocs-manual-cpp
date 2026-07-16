# std::tan(std::valarray)

```cpp
template< class T >
valarray<T> tan( const valarray<T>& va );
```

For each element in `va` computes tangent of the value of the element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing tangents of the values in `va`.

### Notes

Unqualified function (`tan`) is used to perform the computation. If such
function is not available, `std::tan` is used due to argument-dependent lookup.

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
valarray<T> tan(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = tan(i);

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
    const std::valarray<double> y = std::tan(x);
    const std::valarray<double> z = std::atan(y);

    show("x          ", x);
    show("y = tan(x) ", y);
    show("z = atan(y)", z);
}
```

Output:

```text
x           :  0.000000  0.100000  0.200000  0.300000
y = tan(x)  :  0.000000  0.100335  0.202710  0.309336
z = atan(y) :  0.000000  0.100000  0.200000  0.300000
```

### See also

- **sin(std::valarray)** — applies the function `std::sin` to each element of
  valarray (function template)
- **cos(std::valarray)** — applies the function `std::cos` to each element of
  valarray (function template)
- **atan(std::valarray)** — applies the function `std::atan` to each element of
  valarray (function template)
- **tantanftanl (C++11)(C++11)** — computes tangent (\({\small\tan{x}}\)tan(x))
  (function)
- **tan(std::complex)** — computes tangent of a complex number
  (\({\small\tan{z}}\)tan(z)) (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/tan*
