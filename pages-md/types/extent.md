# std::extent

```cpp
template< class T, unsigned N = 0 >
struct extent;  // (since C++11)
```

If `T` is an array type, provides the member constant `value` equal to the
number of elements along the `N`th dimension of the array, if `N` is in
`[``​0​``,``std::rank<T>::value``)`. For any other type, or if `T` is an array
of unknown bound along its first dimension and `N` is `​0​`, `value` is `​0​`.

The behavior of a program that adds specializations for `std::extent` or
`std::extent_v`(since C++17) is undefined.

### Helper variable template

```cpp
template< class T, unsigned N = 0 >
inline constexpr std::size_t extent_v = extent<T, N>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — the number of elements along the `N`th dimension of `T`
  (public static member constant)

### Member functions

- **operator std::size_t** — converts the object to std::size_t, returns `value`
  (public member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — std::size_t
- **`type`** — std::integral_constant<std::size_t, value>

### Possible implementation

```cpp
template<class T, unsigned N = 0>
struct extent : std::integral_constant<std::size_t, 0> {};

template<class T>
struct extent<T[], 0> : std::integral_constant<std::size_t, 0> {};

template<class T, unsigned N>
struct extent<T[], N> : std::extent<T, N - 1> {};

template<class T, std::size_t I>
struct extent<T[I], 0> : std::integral_constant<std::size_t, I> {};

template<class T, std::size_t I, unsigned N>
struct extent<T[I], N> : std::extent<T, N - 1> {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

int main()
{
    static_assert(
        std::extent_v<int[3]> == 3 && //< default dimension is 0
        std::extent_v<int[3], 0> == 3 && //< the same as above
        std::extent_v<int[3][4], 0> == 3 &&
        std::extent_v<int[3][4], 1> == 4 &&
        std::extent_v<int[3][4], 2> == 0 &&
        std::extent_v<int[]> == 0
    );

    const auto ext = std::extent<int['*']>{};
    std::cout << ext << '\n'; //< implicit conversion to std::size_t

    const int ints[]{1, 2, 3, 4};
    static_assert(std::extent_v<decltype(ints)> == 4); //< array size

    [[maybe_unused]] int ary[][3] = {{1, 2, 3}};

    // ary[0] is type of reference of 'int[3]', so, extent
    // cannot calculate correctly and return 0
    static_assert(std::is_same_v<decltype(ary[0]), int(&)[3]>);
    static_assert(std::extent_v<decltype(ary[0])> == 0);
    // removing reference will give correct extent value 3
    static_assert(std::extent_v<std::remove_cvref_t<decltype(ary[0])>> == 3);
}
```

Output:

```text
42
```

### See also

- **is_array (C++11)** — checks if a type is an array type (class template)
- **rank (C++11)** — obtains the number of dimensions of an array type (class
  template)
- **remove_extent (C++11)** — removes one extent from the given array type
  (class template)
- **remove_all_extents (C++11)** — removes all extents from the given array type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/extent*
