# Customization point object

### Explanation

The name `ranges::cpo` denotes a *customization point object*, which is a const
function object of a literal `semiregular` class type. For exposition purposes,
the cv-unqualified version of its type is denoted as `__cpo_fn`.

All instances of `__cpo_fn` are equal. The effects of invoking different
instances of type `__cpo_fn` on the same arguments are equivalent, regardless of
whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `ranges::cpo` can be copied freely and its copies can be
used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `ranges::cpo` above, `__cpo_fn` models

- `std::invocable<__cpo_fn, Args...>`,
- `std::invocable<const __cpo_fn, Args...>`,
- `std::invocable<__cpo_fn&, Args...>`, and
- `std::invocable<const __cpo_fn&, Args...>`.

Otherwise, no function call operator of `__cpo_fn` participates in overload
resolution.

### See also

- RangeAdaptorObject (RAO)
- RangeAdaptorClosureObject (RACO)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cpo*
