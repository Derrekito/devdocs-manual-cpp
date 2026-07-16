# std::ranges::take_while_view<V,Pred>::begin

```cpp
constexpr auto begin() requires (!__SimpleView<V>);  // (1) (since C++20)
constexpr auto begin() const requires
    ranges::range<const V> &&
    std::indirect_unary_predicate<const Pred, ranges::iterator_t<const V>>;  // (2) (since C++20)
```

Returns an iterator to the first element of the view. Effectively calls
`ranges::begin` on the underlying view.

Overload (1) does not participate in overload resolution if `V` is a simple view
(that is, if `V` and `const V` are views with the same iterator and sentinel
types).

### Parameters

(none)

### Return value

`ranges::begin(base_)`, where `base_` is the underlying view.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3450 | C++20 | the `const` overload might return an iterator
      non-comparable to the sentinel | constrained

### See also

- **end (C++20)** — returns a sentinel representing the end (public member
  function)
- **operator== (C++20)** — compares a sentinel with an iterator returned from
  **`take_while_view::begin`** (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_while_view/begin*
