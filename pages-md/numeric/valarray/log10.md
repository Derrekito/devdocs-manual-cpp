# std::log10(std::valarray)

```cpp
template< class T >
valarray<T> log10( const valarray<T>& va );
```

For each element in `va` computes common (base 10) logarithm of the value of the
element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing common logarithms of the values in `va`.

### Notes

Unqualified function (`log10`) is used to perform the computation. If such
function is not available, `std::log10` is used due to argument-dependent
lookup.

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
valarray<T> log10(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = log10(i);

    return other; // proxy object may be returned
}
```

### Example

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>
#include <valarray>

void show(char const* title, const std::valarray<float>& va)
{
    std::cout << title << " : " << std::right;
    for (float x : va)
        std::cout << std::setw(6) << x;
    std::cout << '\n';
}

int main()
{
    const std::valarray<float> n{-2.f, -1.f, 0.f, 1.f, 2.f, 3.f, INFINITY};
    const std::valarray<float> pow10{std::pow(10.f, n)};
    const std::valarray<float> log10_pow10{std::log10(pow10)};

    show("n      ", n);
    show("10ⁿ    ", pow10);
    show("lg(10ⁿ)", log10_pow10);
}
```

Output:

```text
n       :     -2    -1     0     1     2     3   inf
10ⁿ     :   0.01   0.1     1    10   100  1000   inf
lg(10ⁿ) :     -2    -1     0     1     2     3   inf
```

### See also

- **log(std::valarray)** — applies the function `std::log` to each element of
  valarray (function template)
- **log10log10flog10l (C++11)(C++11)** — computes common (base *10*) logarithm
  (\({\small\log_{10}{x}}\)log10(x)) (function)
- **log10(std::complex)** — complex common logarithm with the branch cuts along
  the negative real axis (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/log10*
