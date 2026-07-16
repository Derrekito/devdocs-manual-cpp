# std::negate<void>

```cpp
template<>
class negate<void>;  // (since C++14)
```

`std::negate<>` is a specialization of `std::negate` with parameter and return
type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — returns its negated argument (public member function)

## std::negate<void>::operator()

```cpp
template< class T >
constexpr auto operator()( T&& arg ) const
    -> decltype(-std::forward<T>(arg));
```

Returns the result of negating `arg`.

### Parameters

- **arg** — value to negate

### Return value

`-std::forward<T>(arg)`.

### Example

```cpp
#include <complex>
#include <functional>
#include <iostream>

int main()
{
    auto complex_negate = std::negate<void>{}; // “void” can be omitted
    constexpr std::complex z(4, 2);
    std::cout << z << '\n';
    std::cout << -z << '\n';
    std::cout << complex_negate(z) << '\n';
}
```

Output:

```text
(4,2)
(-4,-2)
(-4,-2)
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/negate_void*
