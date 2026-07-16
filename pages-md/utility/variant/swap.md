# std::variant<Types...>::swap

```cpp
void swap( variant& rhs ) noexcept(/* see below */);  // (since C++17) (until C++20)
constexpr void swap( variant& rhs ) noexcept(/* see below */);  // (since C++20)
```

Swaps two `variant` objects.

- If both `*this` and `rhs` are valueless by exception, does nothing.
- Otherwise, if both `*this` and `rhs` hold the same alternative, calls
  `swap(*std::get_if<i>(this), *std::get_if<i>(std::addressof(rhs)))` where `i`
  is `index()`. If an exception is thrown, the state of the values depends on
  the exception safety of the `swap` function called.
- Otherwise, exchanges values of `rhs` and `*this`. If an exception is thrown,
  the state of `*this` and `rhs` depends on exception safety of variant's move
  constructor.

The behavior is undefined unless lvalues of type `T_i` are Swappable and
`std::is_move_constructible_v<T_i>` is `true` for all `T_i` in `Types...`.

### Parameters

- **rhs** — a `variant` object to swap with

### Return value

(none)

### Exceptions

If `this->index() == rhs.index()`, may throw any exception thrown by
`swap(*std::get_if<i>(this), *std::get_if<i>(std::addressof(rhs)))` with `i`
being `index()`.

Otherwise, may throw any exception thrown by the move constructors of the
alternatives currently held by `*this` and `rhs`.

`noexcept` specification:

`noexcept(((std::is_nothrow_move_constructible_v<Types> &&
std::is_nothrow_swappable_v<Types>) && ...))`

### Example

```cpp
#include <iostream>
#include <string>
#include <variant>

int main()
{
    std::variant<int, std::string> v1{2}, v2{"abc"};
    std::visit([](auto&& x) { std::cout << x << ' '; }, v1);
    std::visit([](auto&& x) { std::cout << x << '\n'; }, v2);
    v1.swap(v2);
    std::visit([](auto&& x) { std::cout << x << ' '; }, v1);
    std::visit([](auto&& x) { std::cout << x << '\n'; }, v2);
}
```

Output:

```text
2 abc
abc 2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2231R1 | C++20 | `swap` was not constexpr while the required operations can
      be constexpr in C++20 | made constexpr

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/swap*
