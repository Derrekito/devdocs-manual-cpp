# std::abs(std::valarray)

```cpp
template< class T >
valarray<T> abs( const valarray<T>& va );
```

Computes absolute value of each element in the value array.

### Parameters

- **va** — value array to apply the operation to

### Return value

Value array containing absolute values of the values in `va`.

### Notes

Unqualified function (`abs`) is used to perform the computation. If such
function is not available, `std::abs` is used due to argument-dependent lookup.

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
valarray<T> abs(const valarray<T>& va)
{
    valarray<T> other = va;
    for (T& i : other)
        i = abs(i);

    return other; // proxy object may be returned
}
```

### Example

```cpp
#include <iostream>
#include <valarray>

int main()
{
    std::valarray<int> v{1, -2, 3, -4, 5, -6, 7, -8};
    std::valarray<int> v2 = std::abs(v);
    for (auto n : v2)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3 4 5 6 7 8
```

### See also

- **abs(int)labsllabs (C++11)** — computes absolute value of an integral value
  (\(\small{|x|}\)|x|) (function)
- **abs(float)fabsfabsffabsl (C++11)(C++11)** — absolute value of a floating
  point value (\(\small{|x|}\)|x|) (function)
- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/abs*
