# operator==(ranges::take_while_view::*sentinel*)

```cpp
friend constexpr bool operator==( const ranges::iterator_t<Base>& x,
                                  const /*sentinel*/& y );  // (since C++20)
```

Compares a `take_while_view::/*sentinel*/` with an iterator into (possibly
const-qualified) view `V`. The iterator is typically obtained from a call to
`take_while_view::begin`.

Returns `true` if `x` compares equal to the underlying sentinel of `y` (i.e.
`y.base()`), or if the predicate returns `false` when applied to `*x`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`take_while_view::sentinel<Const>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare
- **y** — sentinel to compare

### Return value

`y.end_ == x || !std::invoke(*y.pred_, *x)`, where `end_` denotes the stored
sentinel and `pred_` denotes the stored pointer to predicate.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_while_view/sentinel/operator_cmp*
