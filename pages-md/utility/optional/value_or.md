# std::optional<T>::value_or

```cpp
template< class U >
constexpr T value_or( U&& default_value ) const&;  // (1) (since C++17)
template< class U >
constexpr T value_or( U&& default_value ) &&;  // (2) (since C++17)
```

Returns the contained value if `*this` has a value, otherwise returns
`default_value`.

1) Equivalent to `bool(*this) ? **this :
   static_cast<T>(std::forward<U>(default_value))`.

2) Equivalent to `bool(*this) ? std::move(**this) :
   static_cast<T>(std::forward<U>(default_value))`.

### Parameters

- **default_value** — the value to use in case `*this` is empty

**Type requirements**

**-`T` must meet the requirements of CopyConstructible in order to use overload (1).**

**-`T` must meet the requirements of MoveConstructible in order to use overload (2).**

**-`U&&` must be convertible to `T`.**

### Return value

The current value if `*this` has a value, or `default_value` otherwise.

### Exceptions

Any exception thrown by the selected constructor of the return value `T`.

### Example

```cpp
#include <cstdlib>
#include <iostream>
#include <optional>

std::optional<const char*> maybe_getenv(const char* n)
{
    if (const char* x = std::getenv(n))
        return x;
    else
        return {};
}

int main()
{
    std::cout << maybe_getenv("SHELL").value_or("(none)") << '\n';
    std::cout << maybe_getenv("MYPWD").value_or("(none)") << '\n';
}
```

Possible output:

```text
/usr/bin/zsh
(none)
```

### See also

- **value** — returns the contained value (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/value_or*
