# std::plus<void>

```cpp
template<>
class plus<void>;  // (since C++14)
```

std::plus<void> is a specialization of `std::plus` with parameter and return
type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — returns the sum of two arguments (public member function)

## std::plus<void>::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& lhs, U&& rhs ) const
    -> decltype(std::forward<T>(lhs) + std::forward<U>(rhs));
```

Returns the sum of `lhs` and `rhs`.

### Parameters

- **lhs, rhs** — values to sum

### Return value

`std::forward<T>(lhs) + std::forward<U>(rhs)`.

### Example

```cpp
#include <functional>
#include <iostream>

int main()
{
    auto string_plus = std::plus<void>{}; // “void” can be omitted
    std::string a = "Hello ";
    const char* b = "world";
    std::cout << string_plus(a, b) << '\n';
}
```

Output:

```text
Hello world
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/plus_void*
