# std::remove_cvref

```cpp
template< class T >
struct remove_cvref;  // (since C++20)
```

If the type `T` is a reference type, provides the member typedef `type` which is
the type referred to by `T` with its topmost cv-qualifiers removed. Otherwise
`type` is `T` with its topmost cv-qualifiers removed.

The behavior of a program that adds specializations for `std::remove_cvref` is
undefined.

### Member types

- **`type`** — the type referred by `T` or `T` itself if it is not a reference,
  with top-level cv-qualifiers removed

### Helper types

```cpp
template< class T >
using remove_cvref_t = typename remove_cvref<T>::type;  // (since C++20)
```

### Possible implementation

```cpp
template<class T>
struct remove_cvref
{
    typedef std::remove_cv_t<std::remove_reference_t<T>> type;
};
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_remove_cvref` | 201711L | (C++20) | `std::remove_cvref`

### Example

```cpp
#include <type_traits>

int main()
{
    static_assert(std::is_same_v<std::remove_cvref_t<int>, int>);
    static_assert(std::is_same_v<std::remove_cvref_t<int&>, int>);
    static_assert(std::is_same_v<std::remove_cvref_t<int&&>, int>);
    static_assert(std::is_same_v<std::remove_cvref_t<const int&>, int>);
    static_assert(std::is_same_v<std::remove_cvref_t<const int[2]>, int[2]>);
    static_assert(std::is_same_v<std::remove_cvref_t<const int(&)[2]>, int[2]>);
    static_assert(std::is_same_v<std::remove_cvref_t<int(int)>, int(int)>);
}
```

### See also

- **remove_cvremove_constremove_volatile (C++11)(C++11)(C++11)** — removes const
  and/or volatile specifiers from the given type (class template)
- **remove_reference (C++11)** — removes a reference from the given type (class
  template)
- **decay (C++11)** — applies type transformations as when passing a function
  argument by value (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/remove_cvref*
