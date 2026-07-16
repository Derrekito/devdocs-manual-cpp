# operator==(std::common_iterator)

```cpp
template <class I2, std::sentinel_for<I> S2>
    requires std::sentinel_for<S, I2>
friend constexpr bool operator==( const common_iterator& x,
                                  const std::common_iterator<I2, S2>& y );  // (1) (since C++20)
template <class I2, std::sentinel_for<I> S2>
    requires std::sentinel_for<S, I2> && std::equality_comparable_with<I, I2>
friend constexpr bool operator==( const common_iterator& x,
                                  const std::common_iterator<I2, S2>& y );  // (2) (since C++20)
```

Compares the iterators and/or sentinels held by underlying `std::variant` member
objects `var`. Two incomparable iterators or two sentinels are considered equal.

The behavior is undefined if either `x` or `y` is in an invalid state, i.e.
`x.var.valueless_by_exception() || y.var.valueless_by_exception()` is equal to
`true`.

Let `i` be `x.var.index()` and `j` be `y.var.index()`.

1) If `i == j` (i.e. both `x` and `y` hold iterators or both hold sentinels),
   returns `true`, otherwise returns `std::get<i>(x.var) == std::get<j>(y.var)`.

2) If `i == 1 && j == 1` (i.e. both `x` and `y` hold sentinels), returns `true`,
   otherwise returns `std::get<i>(x.var) == std::get<j>(y.var)`.

The `!=` operator is synthesized from `operator==`.

These function templates are not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::common_iterator<I>` is an associated class of the arguments.

### Parameters

- **x, y** — iterator adaptors to compare

### Return value

`true` if underlying iterators and/or sentinels are equal.

### Example

```cpp
#include <cassert>
#include <iterator>

int main()
{
    int a[]{0, 1, 2, 3};

    using CI = std::common_iterator<
                   std::counted_iterator<int*>,
                   std::default_sentinel_t
                   >;

    CI i1{std::counted_iterator{a + 0, 2}};
    CI i2{std::counted_iterator{a + 1, 2}};
    CI i3{std::counted_iterator{a + 0, 3}};
    CI i4{std::counted_iterator{a + 0, 0}};
    CI s1{std::default_sentinel};
    CI s2{std::default_sentinel};

    assert((i1 == i2) == true);
    assert((i1 == i3) == false);
    assert((i2 == i3) == false);
    assert((s1 == s2) == true);
    assert((i1 == s1) == false);
    assert((i4 == s1) == true);
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3574 | C++20 | `variant` was fully constexpr (P2231R1) but
      `common_iterator` was not | also made constexpr

### See also

- **operator- (C++20)** — computes the distance between two iterator adaptors
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/common_iterator/operator_cmp*
