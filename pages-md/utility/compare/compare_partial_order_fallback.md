# std::compare_partial_order_fallback

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */
        compare_partial_order_fallback = /* unspecified */;
}  // (since C++20)
Call signature
template< class T, class U >
    requires /* see below */
constexpr std::partial_ordering
    compare_partial_order_fallback( T&& t, U&& u ) noexcept(/* see below */);  // (since C++20)
```

Performs three-way comparison on subexpressions `t` and `u` and produces a
result of type `std::partial_ordering`, even if the operator `<=>` is
unavailable.

If std::decay_t<T> and std::decay_t<U>> are the same type,
`std::compare_partial_order_fallback(t, u)` is expression-equivalent to:

- `std::partial_order(t, u)`, if it is a well-formed expression;
- otherwise, `t == u ? std::partial_ordering::equivalent :t < u ?
  std::partial_ordering::less :u < t ? std::partial_ordering::greater :
  std::partial_ordering::unordered`

if the expressions `t == u`, `t < u` and `u < t` are all well-formed and each of
decltype(t == u) and decltype(t < u) models `boolean-testable`, except that `t`
and `u` are evaluated only once.

In all other cases, `std::compare_partial_order_fallback(t, u)` is ill-formed,
which can result in substitution failure when it appears in the immediate
context of a template instantiation.

### Customization point objects

The name `std::compare_partial_order_fallback` denotes a *customization point
object*, which is a const function object of a literal `semiregular` class type.
For exposition purposes, the cv-unqualified version of its type is denoted as
`__compare_partial_order_fallback_fn`.

All instances of `__compare_partial_order_fallback_fn` are equal. The effects of
invoking different instances of type `__compare_partial_order_fallback_fn` on
the same arguments are equivalent, regardless of whether the expression denoting
the instance is an lvalue or rvalue, and is const-qualified or not (however, a
volatile-qualified instance is not required to be invocable). Thus,
`std::compare_partial_order_fallback` can be copied freely and its copies can be
used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `std::compare_partial_order_fallback` above,
`__compare_partial_order_fallback_fn` models

- `std::invocable<__compare_partial_order_fallback_fn, Args...>`,
- `std::invocable<const __compare_partial_order_fallback_fn, Args...>`,
- `std::invocable<__compare_partial_order_fallback_fn&, Args...>`, and
- `std::invocable<const __compare_partial_order_fallback_fn&, Args...>`.

Otherwise, no function call operator of `__compare_partial_order_fallback_fn`
participates in overload resolution.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2114 (P2167R3) | C++20 | the fallback mechanism only required return types
      to be convertible to bool | constraints strengthened
  LWG 3465 | C++20 | the fallback mechanism did not require `u < t` to be
      well-formed | required

### See also

- **partial_order (C++20)** — performs 3-way comparison and produces a result of
  type `std::partial_ordering` (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/utility/compare/compare_partial_order_fallback*
