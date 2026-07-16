# std::sin(std::valarray)

```cpp
template< class T >
valarray<T> sin( const valarray<T>& va );
```

For each element in `va` computes sine of the value of the element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing sine of the values in `va`.

### Notes

Unqualified function (`sin`) is used to perform the computation. If such
function is not available, `std::sin` is used due to argument-dependent lookup.

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
valarray<T> sin(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = sin(i);

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
    std::valarray<double> v1 = {0, 0.25, 0.5, 0.75, 1};
    std::valarray<double> v2 = std::sin(v1 * std::numbers::pi);

    for (double n : v2)
        std::cout << std::fixed << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
0.000000 0.707107 1.000000 0.707107 0.000000
```

### See also

- **cos(std::valarray)** — applies the function `std::cos` to each element of
  valarray (function template)
- **tan(std::valarray)** — applies the function `std::tan` to each element of
  valarray (function template)
- **asin(std::valarray)** — applies the function `std::asin` to each element of
  valarray (function template)
- **sinsinfsinl (C++11)(C++11)** — computes sine (\({\small\sin{x}}\)sin(x))
  (function)
- **sin(std::complex)** — computes sine of a complex number
  (\({\small\sin{z}}\)sin(z)) (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/sin*
