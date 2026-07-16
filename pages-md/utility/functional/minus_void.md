# std::minus<void>

```cpp
template<>
class minus<void>;  // (since C++14)
```

std::minus<void> is a specialization of `std::minus` with parameter and return
type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — returns the difference of two arguments (public member
  function)

## std::minus<void>::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& lhs, U&& rhs ) const
    -> decltype(std::forward<T>(lhs) - std::forward<U>(rhs));
```

Returns the difference of `lhs` and `rhs`.

### Parameters

- **lhs, rhs** — values to subtract

### Return value

`std::forward<T>(lhs) - std::forward<U>(rhs)`.

### Example

```cpp
#include <complex>
#include <functional>
#include <iostream>

int main()
{
    auto complex_minus = std::minus<void>{}; // “void” can be omitted
    constexpr std::complex<int> z(4, 2);
    std::cout << complex_minus(z, 1) << '\n';
    std::cout << (z - 1) << '\n';
}
```

Output:

```text
(3,2)
(3,2)
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/minus_void*
