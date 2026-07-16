# std::ranges::greater

```cpp
struct greater;  // (since C++20)
```

Function object for performing comparisons. Deduces the parameter types of the
function call operator from the arguments (but not the return type).

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- **operator()** — checks if the first argument is *greater* than the second
  (public member function)

## std::ranges::greater::operator()

```cpp
template< class T, class U >
constexpr bool operator()( T&& t, U&& u ) const;
```

Equivalent to `return ranges::less{}(std::forward<U>(u), std::forward<T>(t));`.

This overload participates in overload resolution only if
`std::totally_ordered_with<T, U>` is satisfied.

### Notes

Unlike `std::greater`, `std::ranges::greater` requires all six comparison
operators `<`, `<=`, `>`, `>=`, `==` and `!=` to be valid (via the
`totally_ordered_with` constraint) and is entirely defined in terms of
`std::ranges::less`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3530 | C++20 | syntactic checks were relaxed while comparing pointers |
      only semantic requirements are relaxed

### See also

- **greater** — function object implementing `x > y` (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/ranges/greater*
