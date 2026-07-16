# operator==(std::ranges::take_view::*sentinel*<Const>)

```cpp
friend constexpr bool
    operator==( const std::counted_iterator<ranges::iterator_t<Base>>& y,
                const /*sentinel*/& x );  // (1) (since C++20)
template< bool OtherC = !Const >
    requires std::sentinel_for<ranges::sentinel_t<Base>,
                               ranges::iterator_t</*add-const-if*/<OtherC, V>>>
friend constexpr bool operator==( const std::counted_iterator<ranges::iterator_t<
                                      /*add-const-if*/<OtherC, V>>>& y,
                                  const /*sentinel*/& x );  // (2) (since C++20)
```

Compares a `take_view::/*sentinel*/` with a `std::counted_iterator` (typically
obtained from a call to `take_view::begin`).

Returns `true` if `y` points past the N-th element (where N is the number passed
to the constructor of `take_view`), or if end of underlying view is reached.

The exposition-only alias template `add-const-if` is defined as `template<bool
C, class T> using /*add-const-if*/ = std::conditional_t<C, const T, T>;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::take_view::sentinel<Const>` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **y** — `std::counted_iterator` to compare
- **x** — sentinel to compare

### Return value

`y.count() == 0 || y.base() == x.end_`, where `end_` denotes the underlying
sentinel.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3449 | C++20 | comparison between the iterator and the sentinel of
      differently const-qualified `take_view` was unsupported | made supported
      when possible

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_view/sentinel/operator_cmp*
