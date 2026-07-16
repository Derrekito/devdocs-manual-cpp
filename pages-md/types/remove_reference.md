# std::remove_reference

```cpp
template< class T >
struct remove_reference;  // (since C++11)
```

If the type `T` is a reference type, provides the member typedef `type` which is
the type referred to by `T`. Otherwise `type` is `T`.

The behavior of a program that adds specializations for `std::remove_reference`
is undefined.

### Member types

- **`type`** — the type referred by `T` or `T` if it is not a reference

### Helper types

```cpp
template< class T >
using remove_reference_t = typename remove_reference<T>::type;  // (since C++14)
```

### Possible implementation

```cpp
template<class T> struct remove_reference { typedef T type; };
template<class T> struct remove_reference<T&> { typedef T type; };
template<class T> struct remove_reference<T&&> { typedef T type; };
```

### Example

```cpp
#include <iostream>
#include <type_traits>

int main()
{
    std::cout << std::boolalpha;

    std::cout << "std::remove_reference<int>::type is int? "
              << std::is_same<int, std::remove_reference<int>::type>::value << '\n';
    std::cout << "std::remove_reference<int&>::type is int? "
              << std::is_same<int, std::remove_reference<int&>::type>::value << '\n';
    std::cout << "std::remove_reference<int&&>::type is int? "
              << std::is_same<int, std::remove_reference<int&&>::type>::value << '\n';
    std::cout << "std::remove_reference<const int&>::type is const int? "
              << std::is_same<const int,
                              std::remove_reference<const int&>::type>::value << '\n';
}
```

Output:

```text
std::remove_reference<int>::type is int? true
std::remove_reference<int&>::type is int? true
std::remove_reference<int&&>::type is int? true
std::remove_reference<const int&>::type is const int? true
```

### See also

- **is_reference (C++11)** — checks if a type is either an *lvalue reference* or
  *rvalue reference* (class template)
- **add_lvalue_referenceadd_rvalue_reference (C++11)(C++11)** — adds an *lvalue*
  or *rvalue* reference to the given type (class template)
- **remove_cvref (C++20)** — combines `std::remove_cv` and
  `std::remove_reference` (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/remove_reference*
