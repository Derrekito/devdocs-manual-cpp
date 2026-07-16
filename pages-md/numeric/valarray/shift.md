# std::valarray<T>::shift

```cpp
valarray<T> shift( int count ) const;
```

Returns a new valarray of the same size with elements whose positions are
shifted by `count` elements. The new position of each element is i−count where i
is the previous position. The value of shifted in elements is `T()`.

### Parameters

- **count** — number of positions to shift the elements by

### Return value

The resulting valarray with shifted elements.

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

### Example

```cpp
#include <iostream>
#include <valarray>

int main()
{
    std::valarray<int> v{1, 2, 3, 4, 5, 6, 7, 8};

    for (auto const& val : v)
        std::cout << val << ' ';
    std::cout << '\n';

    std::valarray<int> v2 = v.shift(2);

    for (auto const& val : v2)
        std::cout << val << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3 4 5 6 7 8
3 4 5 6 7 8 0 0
```

### See also

- **cshift** — circular shift of the elements of the valarray (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/shift*
