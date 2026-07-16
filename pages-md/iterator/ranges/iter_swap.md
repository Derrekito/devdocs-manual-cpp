# std::ranges::iter_swap

```cpp
namespace ranges {
    inline namespace /* unspecified */ {
        inline constexpr /* unspecified */
            iter_swap = /* unspecified */;
    }
}  // (since C++20) (customization point object)
Call signature
template< class I1, class I2 >
constexpr void iter_swap( I1&& i1, I2&& i2 ) noexcept(/* see below */);  // (since C++20)
Helper function
template< class X, class Y >
constexpr std::iter_value_t<X>
    iter-exchange-move( X&& x, Y&& y )
    noexcept(noexcept(std::iter_value_t<X>(std::ranges::iter_move(x))) &&
             noexcept(*x = std::ranges::iter_move(y)));  // (exposition only*)
```

Swaps values denoted by two iterators.

The effect of the exposition-only helper function `iter-exchange-move` is
equivalent to

```cpp
std::iter_value_t<X> old(std::ranges::iter_move(x));
*x = std::ranges::iter_move(y);
return old;
```

`ranges::iter_swap(i1, i2)` is expression-equivalent to:

1. `(void)iter_swap(i1, i2)`, if `i1` or `i2` has a class or enumeration type
   and the expression is well-formed, where the overload resolution of
   `iter_swap` is performed with the additional candidate `void iter_swap(auto,
   auto) = delete;`[1], excluding `std::ranges::iter_swap` itself. If the
   selected overload does not exchange the value denoted by `i1` and `i2`, the
   program is ill-formed, no diagnostic required.
1. Otherwise, `ranges::swap(*i1, *i2)` if both `I1` and `I2` model
   `indirectly_readable` and if std::iter_reference_t<I1> and
   std::iter_reference_t<I2> model `swappable_with`.
1. Otherwise, `(void)(*i1 = iter-exchange-move(i2, i1))`, if
   `std::indirectly_movable_storable<I1, I2>` and
   `std::indirectly_movable_storable<I2, I1>` are both modeled, except that `i1`
   is only evaluated once.
1. Otherwise, `ranges::iter_swap(i1, i2)` is ill-formed, which can result in
   substitution failure when `ranges::iter_swap(i1, i2)` appears in the
   immediate context of a template instantiation.

1. This precludes calling unconstrained `std::iter_swap`.

### Customization point objects

The name `ranges::iter_swap` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__iter_swap_fn`.

All instances of `__iter_swap_fn` are equal. The effects of invoking different
instances of type `__iter_swap_fn` on the same arguments are equivalent,
regardless of whether the expression denoting the instance is an lvalue or
rvalue, and is const-qualified or not (however, a volatile-qualified instance is
not required to be invocable). Thus, `ranges::iter_swap` can be copied freely
and its copies can be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::iter_swap` above, `__iter_swap_fn` models

- `std::invocable<__iter_swap_fn, Args...>`,
- `std::invocable<const __iter_swap_fn, Args...>`,
- `std::invocable<__iter_swap_fn&, Args...>`, and
- `std::invocable<const __iter_swap_fn&, Args...>`.

Otherwise, no function call operator of `__iter_swap_fn` participates in
overload resolution.

### Example

### See also

- **iter_swap (C++20)** — swaps the objects pointed to by two adjusted
  underlying iterators (function template)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function template)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/ranges/iter_swap*
