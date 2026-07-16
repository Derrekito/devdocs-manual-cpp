# std::conditional

```cpp
template< bool B, class T, class F >
struct conditional;  // (since C++11)
```

Provides member typedef `type`, which is defined as `T` if `B` is `true` at
compile time, or as `F` if `B` is `false`.

The behavior of a program that adds specializations for `std::conditional` is
undefined.

### Member types

- **`type`** — `T` if `B == true`, `F` if `B == false`

### Helper types

```cpp
template< bool B, class T, class F >
using conditional_t = typename conditional<B,T,F>::type;  // (since C++14)
```

### Possible implementation

```cpp
template<bool B, class T, class F>
struct conditional { using type = T; };

template<class T, class F>
struct conditional<false, T, F> { using type = F; };
```

### Example

```cpp
#include <iostream>
#include <type_traits>
#include <typeinfo>

int main()
{
    using Type1 = std::conditional<true, int, double>::type;
    using Type2 = std::conditional<false, int, double>::type;
    using Type3 = std::conditional<sizeof(int) >= sizeof(double), int, double>::type;

    std::cout << typeid(Type1).name() << '\n';
    std::cout << typeid(Type2).name() << '\n';
    std::cout << typeid(Type3).name() << '\n';
}
```

Possible output:

```text
int
double
double
```

### See also

- **enable_if (C++11)** — conditionally removes a function overload or template
  specialization from overload resolution (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/conditional*
