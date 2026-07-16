# std::same_as

```cpp
template< class T, class U >
concept same_as = /* see below */;  // (since C++20)
```

The concept `same_as<T, U>` is satisfied if and only if `T` and `U` denote the
same type.

`std::same_as<T, U>` subsumes `std::same_as<U, T>` and vice versa.

### Possible implementation

```cpp
namespace detail
{
    template< class T, class U >
    concept SameHelper = std::is_same_v<T, U>;
}

template< class T, class U >
concept same_as = detail::SameHelper<T, U> && detail::SameHelper<U, T>;
```

### Example

```cpp
#include <concepts>
#include <iostream>

template<typename T, typename ... U>
concept IsAnyOf = (std::same_as<T, U> || ...);

template<typename T>
concept IsPrintable = std::integral<T> || std::floating_point<T> ||
    IsAnyOf<std::remove_cvref_t<std::remove_pointer_t<std::decay_t<T>>>, char, wchar_t>;

void println(IsPrintable auto const ... arguments)
{
    (std::wcout << ... << arguments) << '\n';
}

int main()
{
    println("Example: ", 3.14, " : ", 42, " : [", 'a', L'-', L"Z]");
}
```

Output:

```text
Example: 3.14 : 42 : [a-Z]
```

### See also

- **is_same (C++11)** — checks if two types are the same (class template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/same_as*
