# std::ranges::iter_move

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */ iter_move = /* unspecified */;
}  // (since C++20) (customization point object)
Call signature
template< class T >
    requires /* see below */
constexpr decltype(auto) iter_move( T&& t ) noexcept(/* see below */);  // (since C++20)
```

Obtains an rvalue reference or a prvalue temporary from a given iterator.

A `ranges::iter_move(t)` is expression-equivalent to:

1. `iter_move(t)`, if `t` has a class or enumeration type and the expression is
   well-formed when treated as an unevaluated operand, where the overload
   resolution of `iter_move` is performed only with the candidates found by
   argument-dependent lookup.
1. Otherwise, `std::move(*t)` if `*t` is well-formed and is an lvalue.
1. Otherwise, `*t` if `*t` is well-formed and is an rvalue.

In all other cases, a call to `ranges::iter_move` is ill-formed, which can
result in substitution failure when `ranges::iter_move(e)` appears in the
immediate context of a template instantiation.

If `ranges::iter_move(e)` is not equal to `*e`, the program is ill-formed, no
diagnostic required.

### Customization point objects

The name `ranges::iter_move` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__iter_move_fn`.

All instances of `__iter_move_fn` are equal. The effects of invoking different
instances of type `__iter_move_fn` on the same arguments are equivalent,
regardless of whether the expression denoting the instance is an lvalue or
rvalue, and is const-qualified or not (however, a volatile-qualified instance is
not required to be invocable). Thus, `ranges::iter_move` can be copied freely
and its copies can be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::iter_move` above, `__iter_move_fn` models

- `std::invocable<__iter_move_fn, Args...>`,
- `std::invocable<const __iter_move_fn, Args...>`,
- `std::invocable<__iter_move_fn&, Args...>`, and
- `std::invocable<const __iter_move_fn&, Args...>`.

Otherwise, no function call operator of `__iter_move_fn` participates in
overload resolution.

### Example

### See also

- **iter_move (C++20)** — casts the result of dereferencing the adjusted
  underlying iterator to its associated rvalue reference type (function)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/ranges/iter_move*
