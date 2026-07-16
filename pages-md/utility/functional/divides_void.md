# std::divides<void>

```cpp
template<>
class divides<void>;  // (since C++14)
```

std::divides<void> is a specialization of `std::divides` with parameter and
return type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — returns the quotient of two arguments (public member
  function)

## std::divides<void>::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& lhs, U&& rhs ) const
    -> decltype(std::forward<T>(lhs) / std::forward<U>(rhs));
```

Returns the quotient of `lhs` and `rhs`.

### Parameters

- **lhs, rhs** — values to divide

### Return value

`std::forward<T>(lhs) / std::forward<U>(rhs)`.

### Example

```cpp
#include <complex>
#include <functional>
#include <iostream>

int main()
{
    auto complex_divides = std::divides<void>{}; // “void” can be omitted
    constexpr std::complex z1{8.0, 4.0}, z2{1.0, 2.0};

    std::cout << std::showpos
              << complex_divides(z1, z2) << ' ' << z1 / z2 << '\n'
              << complex_divides(z1, 5.) << ' ' << z1 / 5. << '\n'
              << complex_divides(6., z2) << ' ' << 6. / z2 << '\n';
}
```

Output:

```text
(+3.2,-2.4) (+3.2,-2.4)
(+1.6,+0.8) (+1.6,+0.8)
(+1.2,-2.4) (+1.2,-2.4)
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/divides_void*
