# std::ranges::take_while_view<V,Pred>::take_while_view

```cpp
take_while_view() requires std::default_initializable<V> &&
                           std::default_initializable<Pred> = default;  // (1) (since C++20)
constexpr explicit take_while_view( V base, Pred pred );  // (2) (since C++20)
```

Constructs a `take_while_view`.

1) Default constructor. Value-initializes the underlying view and the predicate.

2) Move constructs the underlying view from `base` and the predicate from
   `pred`.

### Parameters

- **base** — underlying view
- **fun** — predicate

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3714 (P2711R1) | C++20 | the multi-parameter constructor was not explicit
      | made explicit
  P2325R3 | C++20 | if `Pred` is not `default_initializable`, the default
      constructor constructs a `take_while_view` which does not contain an
      `Pred` | the `take_while_view` is also not `default_initializable`

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_while_view/take_while_view*
