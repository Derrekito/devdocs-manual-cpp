# std::is_eq, std::is_neq, std::is_lt, std::is_gt, std::is_lteq, std::is_gteq

```cpp
constexpr bool is_eq( std::partial_ordering cmp ) noexcept;  // (1) (since C++20)
constexpr bool is_neq( std::partial_ordering cmp ) noexcept;  // (2) (since C++20)
constexpr bool is_lt( std::partial_ordering cmp ) noexcept;  // (3) (since C++20)
constexpr bool is_lteq( std::partial_ordering cmp ) noexcept;  // (4) (since C++20)
constexpr bool is_gt( std::partial_ordering cmp ) noexcept;  // (5) (since C++20)
constexpr bool is_gteq( std::partial_ordering cmp ) noexcept;  // (6) (since C++20)
```

These functions take a result of 3-way comparison and convert it to the result
of one of the six relational operators

Specifically, these functions return

1) `cmp == 0`

2) `cmp != 0`

3) `cmp < 0`

4) `cmp <= 0`

5) `cmp > 0`

6) `cmp >= 0`

### Parameters

- **cmp** — result of 3-way comparison

### Return value

`bool` result of the corresponding relational operation

### Example

### See also

- **partial_ordering (C++20)** — the result type of 3-way comparison that
  supports all 6 operators, is not substitutable, and allows incomparable values
  (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/compare/named_comparison_functions*
