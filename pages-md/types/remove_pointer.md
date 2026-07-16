# std::remove_pointer

```cpp
template< class T >
struct remove_pointer;  // (since C++11)
```

Provides the member typedef `type` which is the type pointed to by `T`, or, if
`T` is not a pointer, then `type` is the same as `T`.

The behavior of a program that adds specializations for `std::remove_pointer` is
undefined.

### Member types

- **`type`** — the type pointed to by `T` or `T` if it's not a pointer

### Helper types

```cpp
template< class T >
using remove_pointer_t = typename remove_pointer<T>::type;  // (since C++14)
```

### Possible implementation

```cpp
template<class T> struct remove_pointer { typedef T type; };
template<class T> struct remove_pointer<T*> { typedef T type; };
template<class T> struct remove_pointer<T* const> { typedef T type; };
template<class T> struct remove_pointer<T* volatile> { typedef T type; };
template<class T> struct remove_pointer<T* const volatile> { typedef T type; };
```

### Example

```cpp
#include <type_traits>

static_assert
(
    std::is_same_v<int, int> == true &&
    std::is_same_v<int, int*> == false &&
    std::is_same_v<int, int**> == false &&
    std::is_same_v<int, std::remove_pointer_t<int>> == true &&
    std::is_same_v<int, std::remove_pointer_t<int*>> == true &&
    std::is_same_v<int, std::remove_pointer_t<int**>> == false &&
    std::is_same_v<int, std::remove_pointer_t<int* const>> == true &&
    std::is_same_v<int, std::remove_pointer_t<int* volatile>> == true &&
    std::is_same_v<int, std::remove_pointer_t<int* const volatile>> == true
);

int main() {}
```

### See also

- **is_pointer (C++11)** — checks if a type is a pointer type (class template)
- **add_pointer (C++11)** — adds a pointer to the given type (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/remove_pointer*
