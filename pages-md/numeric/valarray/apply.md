# std::valarray<T>::apply

```cpp
valarray<T> apply( T func(T) ) const;
valarray<T> apply( T func(const T&) ) const;
```

Returns a new valarray of the same size with values which are acquired by
applying function `func` to the previous values of the elements.

### Parameters

- **func** — function to apply to the values

### Return value

The resulting valarray with values acquired by applying function `func`.

### Notes

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

Following straightforward implementations can be replaced by expression
templates for a higher efficiency.

```cpp
template<class T>
valarray<T> valarray<T>::apply(T func(T)) const
{
    valarray<T> other = *this;
    for (T& i : other)
        i = func(i);
    return other;
}

template<class T>
valarray<T> valarray<T>::apply(T func(const T&)) const
{
    valarray<T> other = *this;
    for (T& i : other)
        i = func(i);
    return other;
}
```

### Example

Calculates and prints the first 10 factorials.

```cpp
#include <cmath>
#include <iostream>
#include <valarray>

int main()
{
    std::valarray<int> v = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    v = v.apply([](int n) -> int
                {
                    return std::round(std::tgamma(n + 1));
                });
    for (auto n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 6 24 120 720 5040 40320 362880 3628800
```

### See also

- **for_each** — applies a function to a range of elements (function template)
- **ranges::for_each (C++20)** — applies a function to a range of elements
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/apply*
