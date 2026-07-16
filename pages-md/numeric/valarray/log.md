# std::log(std::valarray)

```cpp
template< class T >
valarray<T> log( const valarray<T>& va );
```

For each element in `va` computes natural logarithm of the value of the element.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing natural logarithms of the values in `va`.

### Notes

Unqualified function (`log`) is used to perform the computation. If such
function is not available, `std::log` is used due to argument-dependent lookup.

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
valarray<T> log(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = log(i);

    return other; // proxy object may be returned
}
```

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <valarray>

void show(char const* title, const std::valarray<double>& va)
{
    std::cout << title << " :" << std::right << std::fixed;
    for (double x : va)
        std::cout << std::setw(10) << x;
    std::cout << '\n';
}

int main()
{
    const std::valarray<double> n{0.0, 1.0, 2.0, 3.0};
    const std::valarray<double> exp_n{std::exp(n)};
    const std::valarray<double> log_exp_n{std::log(exp_n)};

    show("n      ", n);
    show("eⁿ     ", exp_n);
    show("log(eⁿ)", log_exp_n);
}
```

Output:

```text
n       :  0.000000  1.000000  2.000000  3.000000
eⁿ      :  1.000000  2.718282  7.389056 20.085537
log(eⁿ) :  0.000000  1.000000  2.000000  3.000000
```

### See also

- **log10(std::valarray)** — applies the function `std::log10` to each element
  of valarray (function template)
- **exp(std::valarray)** — applies the function `std::exp` to each element of
  valarray (function template)
- **loglogflogl (C++11)(C++11)** — computes natural (base *e*) logarithm
  (\({\small\ln{x}}\)ln(x)) (function)
- **log(std::complex)** — complex natural logarithm with the branch cuts along
  the negative real axis (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/log*
