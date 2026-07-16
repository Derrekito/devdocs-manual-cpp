# std::ranges::common_view<V>::common_view

```cpp
common_view() = default;  // (1) (since C++20)
constexpr explicit common_view( V r );  // (2) (since C++20)
```

Constructs a `common_view`.

1) Default constructor. Value-initializes the underlying view. After
   construction, `base()` returns a copy of `V()`.

2) Initializes the underlying view with `std::move(r)`.

### Parameters

- **r** — underlying view to be adapted into a common-range

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3405 | C++20 | the redundant converting constructor might cause constraint
      recursion | removed

---
*Source: https://en.cppreference.com/w/cpp/ranges/common_view/common_view*
