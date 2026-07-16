# std::ranges::equal_to

```cpp
struct equal_to;  // (since C++20)
```

Function object for performing comparisons. The parameter types of the function
call operator (but not the return type) are deduced from the arguments.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- **operator()** — checks if the arguments are *equal* (public member function)

## std::ranges::equal_to::operator()

```cpp
template< class T, class U >
constexpr bool operator()( T&& t, U&& u ) const;
```

Given the expression `std::forward<T>(t) == std::forward<U>(u)` as `expr`:

- If `expr` results in a call to built-in operator== comparing pointers, given
  the composite pointer type of `t` and `u` as `P`:
- Otherwise:

This overload participates in overload resolution only if
`std::equality_comparable_with<T, U>` is satisfied.

### Notes

Compared to `std::equal_to`, `std::ranges::equal_to` additionally requires `!=`
to be valid, and that both argument types are required to be (homogeneously)
comparable with themselves (via the `equality_comparable_with` constraint).

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3530 | C++20 | syntactic checks were relaxed while comparing pointers |
      only semantic requirements are relaxed

### See also

- **equal_to** — function object implementing `x == y` (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/ranges/equal_to*
