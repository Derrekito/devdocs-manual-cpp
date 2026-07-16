# std::partial_order

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */ partial_order = /* unspecified */;
}  // (since C++20)
Call signature
template< class T, class U >
    requires /* see below */
constexpr std::partial_ordering
    partial_order( T&& t, U&& u ) noexcept(/* see below */);
```

Compares two values using 3-way comparison and produces a result of type
`std::partial_ordering`.

Let `t` and `u` be expressions and `T` and `U` denote decltype((t)) and
decltype((u)) respectively, `std::partial_order(t, u)` is expression-equivalent
to:

- If `std::is_same_v<std::decay_t<T>, std::decay_t<U>>` is `true`:
  `std::partial_ordering(partial_order(t, u))`, if it is a well-formed
  expression with overload resolution performed in a context that does not
  include a declaration of `std::partial_order`, otherwise,
  `std::partial_ordering(std::compare_three_way()(t, u))`, if it is well-formed,
  otherwise, `std::partial_ordering(std::weak_order(t, u))`, if it is
  well-formed.
- In all other cases, the expression is ill-formed, which can result in
  substitution failure when it appears in the immediate context of a template
  instantiation.

### Customization point objects

The name `std::partial_order` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as
`__partial_order_fn`.

All instances of `__partial_order_fn` are equal. The effects of invoking
different instances of type `__partial_order_fn` on the same arguments are
equivalent, regardless of whether the expression denoting the instance is an
lvalue or rvalue, and is const-qualified or not (however, a volatile-qualified
instance is not required to be invocable). Thus, `std::partial_order` can be
copied freely and its copies can be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `std::partial_order` above, `__partial_order_fn`
models

- `std::invocable<__partial_order_fn, Args...>`,
- `std::invocable<const __partial_order_fn, Args...>`,
- `std::invocable<__partial_order_fn&, Args...>`, and
- `std::invocable<const __partial_order_fn&, Args...>`.

Otherwise, no function call operator of `__partial_order_fn` participates in
overload resolution.

### Example

### See also

- **partial_ordering (C++20)** — the result type of 3-way comparison that
  supports all 6 operators, is not substitutable, and allows incomparable values
  (class)
- **strong_order (C++20)** — performs 3-way comparison and produces a result of
  type `std::strong_ordering` (customization point object)
- **weak_order (C++20)** — performs 3-way comparison and produces a result of
  type `std::weak_ordering` (customization point object)
- **compare_partial_order_fallback (C++20)** — performs 3-way comparison and
  produces a result of type `std::partial_ordering`, even if `operator<=>` is
  unavailable (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/utility/compare/partial_order*
