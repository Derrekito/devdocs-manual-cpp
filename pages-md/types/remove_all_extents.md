# std::remove_all_extents

```cpp
template< class T >
struct remove_all_extents;  // (since C++11)
```

If `T` is a multidimensional array of some type `X`, provides the member typedef
`type` equal to `X`, otherwise `type` is `T`.

The behavior of a program that adds specializations for
`std::remove_all_extents` is undefined.

### Member types

- **`type`** — the type of the element of `T`

### Helper types

```cpp
template< class T >
using remove_all_extents_t = typename remove_all_extents<T>::type;  // (since C++14)
```

### Possible implementation

```cpp
template<class T>
struct remove_all_extents { typedef T type; };

template<class T>
struct remove_all_extents<T[]> {
    typedef typename remove_all_extents<T>::type type;
};

template<class T, std::size_t N>
struct remove_all_extents<T[N]> {
    typedef typename remove_all_extents<T>::type type;
};
```

### Example

```cpp
#include <iostream>
#include <type_traits>
#include <typeinfo>

template<class A>
void info(const A&)
{
    typedef typename std::remove_all_extents<A>::type Type;
    std::cout << "underlying type: " << typeid(Type).name() << '\n';
}

int main()
{
    float a0;
    float a1[1][2][3];
    float a2[1][1][1][1][2];
    float* a3;
    int a4[3][2];
    double a5[2][3];
    struct X { int m; } x0[3][3];

    info(a0);
    info(a1);
    info(a2);
    info(a3);
    info(a4);
    info(a5);
    info(x0);
}
```

Possible output:

```text
underlying type: float
underlying type: float
underlying type: float
underlying type: float*
underlying type: int
underlying type: double
underlying type: main::X
```

### See also

- **is_array (C++11)** — checks if a type is an array type (class template)
- **rank (C++11)** — obtains the number of dimensions of an array type (class
  template)
- **extent (C++11)** — obtains the size of an array type along a specified
  dimension (class template)
- **remove_extent (C++11)** — removes one extent from the given array type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/remove_all_extents*
