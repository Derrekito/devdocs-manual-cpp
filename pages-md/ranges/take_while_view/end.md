# std::ranges::take_while_view<V,Pred>::end

```cpp
constexpr auto end() requires (!__SimpleView<V>);  // (1) (since C++20)
constexpr auto end() const requires
    ranges::range<const V> &&
    std::indirect_unary_predicate<const Pred, ranges::iterator_t<const V>>;  // (2) (since C++20)
```

Returns a sentinel representing the end of the view.

Let `base_` denote the underlying view.

1) Effectively returns `/*sentinel*/<false>(ranges::end(base_),
   std::addressof(pred()))`.

2) Effectively returns `/*sentinel*/<true>(ranges::end(base_),
   std::addressof(pred()))`.

Overload (1) does not participate in overload resolution if `V` is a simple view
(that is, if `V` and `const V` are views with the same iterator and sentinel
types).

### Parameters

(none)

### Return value

A sentinel representing the end of the view.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3450 | C++20 | the `const` overload might return a sentinel non-comparable
      to the iterator | constrained

### See also

- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **operator== (C++20)** — compares a sentinel with an iterator returned from
  `take_while_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_while_view/end*
