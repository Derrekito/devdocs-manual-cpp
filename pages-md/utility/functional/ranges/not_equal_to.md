# std::ranges::not_equal_to

```cpp
struct not_equal_to;  // (since C++20)
```

Function object for performing comparisons. Deduces the parameter types of the
function call operator from the arguments (but not the return type).

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- **operator()** — checks if the arguments are *not equal* (public member
  function)

## std::ranges::not_equal_to::operator()

```cpp
template< class T, class U >
constexpr bool operator()( T&& t, U&& u ) const;
```

Equivalent to `return !ranges::equal_to{}(std::forward<T>(t),
std::forward<U>(u));`.

This overload participates in overload resolution only if
`std::equality_comparable_with<T, U>` is satisfied.

### Notes

Unlike `std::not_equal_to`, `std::ranges::not_equal_to` requires both `==` and
`!=` to be valid (via the `equality_comparable_with` constraint), and is
entirely defined in terms of `std::ranges::equal_to`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3530 | C++20 | syntactic checks were relaxed while comparing pointers |
      only semantic requirements are relaxed

### See also

- **not_equal_to** — function object implementing `x != y` (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/ranges/not_equal_to*
