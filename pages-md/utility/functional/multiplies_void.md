# std::multiplies<void>

```cpp
template<>
class multiplies<void>;  // (since C++14)
```

std::multiplies<void> is a specialization of `std::multiplies` with parameter
and return type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — returns the product of two arguments (public member
  function)

## std::multiplies<void>::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& lhs, U&& rhs ) const
    -> decltype(std::forward<T>(lhs) * std::forward<U>(rhs));
```

Returns the product of `lhs` and `rhs`.

### Parameters

- **lhs, rhs** — values to multiply

### Return value

`std::forward<T>(lhs) * std::forward<U>(rhs)`.

### Example

```cpp
#include <complex>
#include <functional>
#include <iostream>

int main()
{
    auto complex_multiplies = std::multiplies<void>{}; // “void” can be omitted
    constexpr std::complex z1{1.0, 2.0}, z2{3.0, 4.0};

    std::cout << std::showpos
              << complex_multiplies(z1, z2) << ' ' << z1 * z2 << '\n'
              << complex_multiplies(z1, 5.) << ' ' << z1 * 5. << '\n'
              << complex_multiplies(5., z1) << ' ' << 5. * z1 << '\n';
}
```

Output:

```text
(-5,+10) (-5,+10)
(+5,+10) (+5,+10)
(+5,+10) (+5,+10)
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/multiplies_void*
