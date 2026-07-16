# std::modulus<void>

```cpp
template<>
class modulus<void>;  // (since C++14)
```

std::modulus<void> is a specialization of `std::modulus` with parameter and
return type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — returns the modulus of two arguments (public member
  function)

## std::modulus<void>::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& lhs, U&& rhs ) const
    -> decltype(std::forward<T>(lhs) % std::forward<U>(rhs));
```

Returns the remainder of the division of `lhs` by `rhs`.

### Parameters

- **lhs, rhs** — values to divide

### Return value

`std::forward<T>(lhs) % std::forward<U>(rhs)`.

### Example

```cpp
#include <functional>
#include <iostream>

struct M
{
    M(int x) { std::cout << "M(" << x << ");\n"; }
    M() {}
};

auto operator%(M, M) { std::cout << "operator%(M, M);\n"; return M{}; }
auto operator%(M, int) { std::cout << "operator%(M, int);\n"; return M{}; }
auto operator%(int, M) { std::cout << "operator%(int, M);\n"; return M{}; }

int main()
{
    M m;

    // 42 is converted into a temporary object M{42}
    std::modulus<M>{}(m, 42);    // calls operator%(M, M)

    // no temporary object
    std::modulus<void>{}(m, 42); // calls operator%(M, int)
    std::modulus<void>{}(42, m); // calls operator%(int, M)
}
```

Output:

```text
M(42);
operator%(M, M);
operator%(M, int);
operator%(int, M);
```

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/modulus_void*
