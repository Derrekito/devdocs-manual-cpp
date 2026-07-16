# std::swap(std::optional)

```cpp
template< class T >
void swap( std::optional<T>& lhs,
           std::optional<T>& rhs ) noexcept(/* see below */);  // (since C++17) (until C++20)
template< class T >
constexpr void swap( std::optional<T>& lhs,
                     std::optional<T>& rhs ) noexcept(/* see below */);  // (since C++20)
```

Overloads the `std::swap` algorithm for `std::optional`. Exchanges the state of
`lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

This overload participates in overload resolution only if
`std::is_move_constructible_v<T>` and `std::is_swappable_v<T>` are both `true`.

### Parameters

- **lhs, rhs** — `optional` objects whose states to swap

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept(noexcept(lhs.swap(rhs)))`

### Example

```cpp
#include <iostream>
#include <optional>
#include <string>

int main()
{
    std::optional<std::string> a{"██████"}, b{"▒▒▒▒▒▒"};

    auto print = [&](auto const& s)
    {
        std::cout
            << s << '\t'
            << "a = " << a.value_or("(null)") << "  "
            << "b = " << b.value_or("(null)") << '\n';
    };

    print("Initially:");
    std::swap(a, b);
    print("swap(a, b):");
    a.reset();
    print("\n""a.reset():");
    std::swap(a, b);
    print("swap(a, b):");
}
```

Output:

```text
Initially:        a = ██████  b = ▒▒▒▒▒▒
swap(a, b):        a = ▒▒▒▒▒▒  b = ██████

a.reset():        a = (null)  b = ██████
swap(a, b):        a = ██████  b = (null)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2231R1 | C++20 | `swap` was not constexpr while the required operations can
      be constexpr in C++20 | made constexpr

### See also

- **swap** — exchanges the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/swap2*
