# std::ranges::less

```cpp
struct less;  // (since C++20)
```

Function object for performing comparisons. Deduces the parameter types of the
function call operator from the arguments (but not the return type).

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- **operator()** — checks if the first argument is *less* than the second
  (public member function)

## std::ranges::less::operator()

```cpp
template< class T, class U >
constexpr bool operator()( T&& t, U&& u ) const;
```

Given the expression `std::forward<T>(t) < std::forward<U>(u)` as `expr`:

- If `expr` results in a call to built-in operator< comparing pointers, given
  the composite pointer type of `t` and `u` as `P`:
- Otherwise:

This overload participates in overload resolution only if
`std::totally_ordered_with<T, U>` is satisfied.

If there exists an expression `expr1` of type `T` and an expression `expr2` of
`U`, such that the comparison results of `expr1` and `expr2` violate strict
total ordering (rules are defined below), the behavior is undefined.

The comparison results of `expr1` and `expr2` follow strict total ordering only
if **exactly one** of the following expressions is `true`:

- `std::ranges::less{}(expr1, expr2)`
- `std::ranges::less{}(expr2, expr1)`
- `std::ranges::equal_to{}(expr1, expr2)`

### Notes

Unlike `std::less`, `std::ranges::less` requires all six comparison operators
`<`, `<=`, `>`, `>=`, `==` and `!=` to be valid (via the `totally_ordered_with`
constraint).

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3530 | C++20 | syntactic checks were relaxed while comparing pointers |
      only semantic requirements are relaxed

### See also

- **less** — function object implementing `x < y` (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/ranges/less*
