# std::remove_extent

```cpp
template< class T >
struct remove_extent;  // (since C++11)
```

If `T` is an array of some type `X`, provides the member typedef `type` equal to
`X`, otherwise `type` is `T`. Note that if T is a multidimensional array, only
the first dimension is removed.

The behavior of a program that adds specializations for `std::remove_extent` is
undefined.

### Member types

- **`type`** — the type of the element of `T`

### Helper types

```cpp
template< class T >
using remove_extent_t = typename remove_extent<T>::type;  // (since C++14)
```

### Possible implementation

```cpp
template<class T>
struct remove_extent { using type = T; };

template<class T>
struct remove_extent<T[]> { using type = T; };

template<class T, std::size_t N>
struct remove_extent<T[N]> { using type = T; };
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <type_traits>

template<class A>
    std::enable_if_t<std::rank_v<A> == 1>
print_1d(const A& a)
{
    std::copy(a, a + std::extent_v<A>,
        std::ostream_iterator<std::remove_extent_t<A>>(std::cout, " "));
    std::cout << '\n';
}

int main()
{
    int a[][3] = {{1, 2, 3}, {4, 5, 6}};
//  print_1d(a); // compile-time error
    print_1d(a[1]);
}
```

Output:

```text
4 5 6
```

### See also

- **is_array (C++11)** — checks if a type is an array type (class template)
- **rank (C++11)** — obtains the number of dimensions of an array type (class
  template)
- **extent (C++11)** — obtains the size of an array type along a specified
  dimension (class template)
- **remove_all_extents (C++11)** — removes all extents from the given array type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/remove_extent*
