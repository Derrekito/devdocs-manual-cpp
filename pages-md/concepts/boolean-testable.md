# *boolean-testable*

```cpp
template< class B >
concept __boolean_testable_impl = std::convertible_to<B, bool>;  // (exposition only*)
template< class B >
concept boolean-testable =
    __boolean_testable_impl<B> &&
    requires (B&& b) {
        { !std::forward<B>(b) } -> __boolean_testable_impl;
    };  // (exposition only*)
```

The exposition-only concept `boolean-testable` specifies the requirements for
expressions that are convertible to `bool` and for which the logical operators
have the usual behavior (including short-circuiting), even for two different
`boolean-testable` types.

Formally, to model the exposition-only concept `__boolean_testable_impl`, the
type must not define any member `operator&&` and `operator||`, and no viable
non-member `operator&&` and `operator||` may be visible by argument-dependent
lookup. Additionally, given an expression `e` such that `decltype((e))` is `B`,
`boolean-testable` is modeled only if `bool(e) == !bool(!e)`.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### Notes

Examples of `boolean-testable` types include `bool`, `std::true_type`,
`std::bitset<N>::reference`, and `int*`.

---
*Source: https://en.cppreference.com/w/cpp/concepts/boolean-testable*
