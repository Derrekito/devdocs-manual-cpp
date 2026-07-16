# std::type_identity

```cpp
template< class T >
struct type_identity;  // (since C++20)
```

Provides the member typedef `type` that names `T` (i.e., the identity
transformation).

The behavior of a program that adds specializations for `std::type_identity` is
undefined.

### Member types

- **`type`** — `T`

### Helper types

```cpp
template< class T >
using type_identity_t = typename type_identity<T>::type;  // (since C++20)
```

### Possible implementation

```cpp
template<class T>
struct type_identity { using type = T; };
```

### Notes

`std::type_identity` can be used to establish non-deduced contexts in template
argument deduction.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_type_identity` | 201806L | (C++20) | `std::type_identity`

### Example

```cpp
#include <iostream>
#include <type_traits>

template<class T>
T foo(T a, T b) { return a + b; }

template<class T>
T bar(T a, std::type_identity_t<T> b) { return a + b; }

int main()
{
    // foo(4.2, 1); // error, deduced conflicting types for 'T'
    std::cout << bar(4.2, 1) << '\n';  // OK, calls bar<double>
}
```

Output:

```text
5.2
```

---
*Source: https://en.cppreference.com/w/cpp/types/type_identity*
